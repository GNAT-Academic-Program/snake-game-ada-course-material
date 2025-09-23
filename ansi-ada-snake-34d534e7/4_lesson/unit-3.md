---
kind: unit

title: Handle Specific Input Commands

name: 3-snake
---

Let's filter input to handle only the commands we need! We'll support Enter/Space to select and Q/Esc to quit.

1. **Add command type** to `noki.ads` after `procedure Log (S : String);`:
   ```ada
   type Cmd_T is (Enter, Quit, Undefined);
   ```

2. **Add protected object** for thread-safe command sharing:
   ```ada
   protected type Input_Cmd_T is
      procedure Set (Cmd : Cmd_T);
      function Get return Cmd_T;
      procedure Reset;
   private
      Local_Cmd : Cmd_T := Undefined;
   end Input_Cmd_T;

   Input_Cmd : Input_Cmd_T;
   ```

## 📋 **Code Review - Complete `noki.ads`**

Your complete `noki.ads` should look like this:

```ada
package Noki is
   CSI : constant String := Character'Val (16#1B#) & '[';
   function Clear_Screen return String is (CSI & "2J" & CSI & "H");

   procedure Log (S : String);

   type Cmd_T is (Enter, Quit, Undefined);

   protected type Input_Cmd_T is
      procedure Set (Cmd : Cmd_T);
      function Get return Cmd_T;
      procedure Reset;
   private
      Local_Cmd : Cmd_T := Undefined;
   end Input_Cmd_T;

   Input_Cmd : Input_Cmd_T;

   task type Input_Task_T;

end Noki;
```

3. **Implement protected object** in `noki.adb` before the task body:
   ```ada
   protected body Input_Cmd_T is
      procedure Set (Cmd : Cmd_T) is
      begin
         Local_Cmd := Cmd;
      end Set;
      
      function Get return Cmd_T is (Local_Cmd);

      procedure Reset is
      begin
         Local_Cmd := Undefined;
      end Reset;
   end Input_Cmd_T;
   ```

4. **Update task body** - replace the character logging with command setting:
   ```ada
   -- Replace the Ada.Text_IO.Get_Immediate section with:
   Ada.Text_IO.Get_Immediate (C);
   case C is
      when 'q' | Character'Val (27) => 
         Input_Cmd.Set (Quit);
      when Character'Val (10) | Character'Val (32) => 
         Input_Cmd.Set (Enter);
      when others => 
         Input_Cmd.Set (Undefined);
   end case;
   ```

## 📋 **Code Review - Complete `noki.adb`**

Your complete `noki.adb` should look like this:

```ada
with Ada.Text_IO;

package body Noki is

   procedure Log (S : String) is
   begin
      Ada.Text_IO.Put (S);
      Ada.Text_IO.New_Line;
   end Log;

   protected body Input_Cmd_T is
      procedure Set (Cmd : Cmd_T) is
      begin
         Local_Cmd := Cmd;
      end Set;
      
      function Get return Cmd_T is (Local_Cmd);

      procedure Reset is
      begin
         Local_Cmd := Undefined;
      end Reset;
   end Input_Cmd_T;

   task body Input_Task_T is
      C : Character;
   begin
      loop
         Ada.Text_IO.Get_Immediate (C);
         case C is
            when 'q' | Character'Val (27) =>
               Input_Cmd.Set (Quit);
            when Character'Val (10) | Character'Val (32) =>
               Input_Cmd.Set (Enter);
            when others =>
               Input_Cmd.Set (Undefined);
         end case;
      end loop;
   end Input_Task_T;

end Noki;
```

5. **Update main game** in `snake_game.adb`:
   - Add `with GNAT.OS_Lib;` at the top
   - Add `use N;` after the package rename for operator visibility
   - Add exit condition: `exit when Input_Cmd.Get = Quit;` after `loop`
   - Update Welcome state to check for Enter command
   - Update Game_Over state to handle replay functionality
   - Add `GNAT.OS_Lib.OS_Exit (0);` after the loop ends

**Key changes to `snake_game.adb`:**
```ada
-- Add at top:
with GNAT.OS_Lib;

-- After package rename:
use N;

-- Add after 'loop':
exit when Input_Cmd.Get = Quit;

-- Update Welcome state:
when Welcome =>
   Log ("Welcome to Snakotron!");
   Log ("Press Enter/Space to play, Q/Esc to quit");
   if Input_Cmd.Get = Enter then
      Game_State := Play;
      Input_Cmd.Reset;
   end if;

-- Update Game_Over state:
when Game_Over =>
   Log ("Game Over!");
   Log ("Press Enter/Space to replay");
   if Input_Cmd.Get = Enter then
      Game_State := Play;
      Input_Cmd.Reset;
   end if;

-- Add after 'end loop;':
GNAT.OS_Lib.OS_Exit (0);
```

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete `snake_game.adb` should look like this:

```ada
with Noki;
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

✅ **Expected result:** Game responds to Enter/Space (advance/replay) and Q/Esc (quit) keys only. Players can replay after Game Over. Pressing Q/Esc properly terminates the program.

::remark-box
---
kind: info
---
🤯 **Heads Up!** Protected objects handle concurrent access automatically - the runtime queues access calls to prevent data corruption between threads.
::
