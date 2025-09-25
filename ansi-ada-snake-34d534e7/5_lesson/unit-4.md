---
kind: unit

title: Draw Pixels to Terminal

name: 4-snake
---

Let's create a Draw procedure to output our pixel to the terminal! We'll use Unicode I/O to handle our Wide_Wide_String pixel data.

## Understanding Unicode I/O in Ada

Since we're dealing with Unicode characters and Wide_Wide_String, we need different I/O methods than regular strings. Ada provides `Ada.Wide_Wide_Text_IO` for this purpose.

## 1. Declare the Draw Procedure

Edit `noki.ads` and add this procedure signature after the current `Log` definition:

```ada
procedure Draw (P : Pixel_T);
```

## 2. Add Unicode I/O Support

Edit `noki.adb` and add this `with` clause at the top:

```ada
with Ada.Wide_Wide_Text_IO;
```

**Why?** We need the standard I/O library for Unicode to output our Wide_Wide_String pixel data.

## 3. Implement the Draw Procedure

Add this implementation in `noki.adb`:

```ada
procedure Draw (P : Pixel_T) is
begin
   Ada.Wide_Wide_Text_IO.Put (+P);
end Draw;
```

::remark-box
---
kind: info
---
🤯 **Heads Up!** Make sure the `Draw` implementation comes **after** the `+` function implementation, otherwise the compiler won't find the `+` operator when we use `+P`.
::

**Key insight:** We're calling our `+` function from the previous lesson to convert the `Pixel_T` to `WWStr_T` in one clean operation, then outputting it with Unicode I/O.

## 4. Display Snake Pixels in Game

Now let's show our snake during gameplay! Edit `snake_game.adb` and replace the `Play` state code:

```ada
when Play =>
   Log ("We are playing.");
   Draw (Tongue_Pix);
   Draw (Head_Pix);
   Draw (Torso_Pix);
   Draw (Torso_Pix);
   Draw (Torso_Pix);
   Draw (Torso_Pix);
   Game_State := Game_Over;
```

## 5. Import the Snake Package

Since we defined our game-specific pixels (`Tongue_Pix`, `Head_Pix`, `Torso_Pix`) in the Snake package, we need to import it in our main program.

Add this line at the top of `snake_game.adb`:

```ada
with Snake; use Snake;
```

::remark-box
---
kind: info
---
🤯 **Heads Up!** The `use` directive makes package resources directly accessible. Since we have `package N renames Noki; use N;`, we can call `Log`, `Draw`, etc. directly without the `N.` prefix. Same with `with Snake; use Snake;`.
::

## 📋 **Code Review - Complete snake_game.adb**

Your `main` should now look like this:

```ada
with Noki;
with Snake; use Snake;
with GNAT.OS_Lib;

procedure Snake_Game is
   package N renames Noki;
   use N;
   Input_Task : Input_Task_T;
   type Game_State_T is (Welcome, Play, Game_Over, Undefined);
   Game_State : Game_State_T := Welcome;
begin
   loop
      exit when Input_Cmd.Get = Quit;
      Log (Clear_Screen);
      case Game_State is
         when Welcome =>
            Log ("Welcome to Snakotron!");
            Log ("Press Enter/Space to play, Q/Esc to quit");
            if Input_Cmd.Get = Enter then
               Game_State := Play;
               Input_Cmd.Reset;
            end if;
         when Play =>
            Log ("We are playing.");
            Draw (Tongue_Pix);
            Draw (Head_Pix);
            Draw (Torso_Pix);
            Draw (Torso_Pix);
            Draw (Torso_Pix);
            Draw (Torso_Pix);
            Game_State := Game_Over;
         when Game_Over =>
            Log ("Game Over!");
            Log ("Press Enter/Space to replay");
            if Input_Cmd.Get = Enter then
               Game_State := Play;
               Input_Cmd.Reset;
            end if;
         when others =>
            null;
      end case;
      delay 1.0;
   end loop;
   GNAT.OS_Lib.OS_Exit (0);
end Snake_Game;
```

✅ **Expected result:** Your snake pixels now appear on screen with full color and Unicode characters support during the Play state!
