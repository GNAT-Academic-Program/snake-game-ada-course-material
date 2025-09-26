---
kind: unit

title: Create a Custom Game State Type

name: 3-snake
---

Let's create our first user-defined Ada type! We'll use an enumeration to manage different game states.

1. **Add the type definition** to the **declarative part** of the Snake_Game procedure:
   - After `procedure Snake_Game is` and before `begin`, add:
   ```ada
   type Game_State_T is (Welcome, Play, Game_Over, Undefined);
   Game_State : Game_State_T := Welcome;
   ```

2. **Understanding the syntax:**
   - First line creates a new enumeration type `Game_State_T` with four possible values
   - Second line declares a variable `Game_State` of type `Game_State_T`, initialized to `Welcome`
   - Ada syntax: `variable_name : variable_type := initial_value`

3. **Replace the simple log with state handling:**
   - Remove the `Noki.Log ("Welcome to my Snake Game!");` line
   - Add a case statement between `loop` and `end loop`:
   ```ada
   case Game_State is
      when Welcome =>
         Noki.Log ("Welcome to Snakotron!");
         Game_State := Play;
      when Play =>
         Noki.Log ("We are playing.");
         Game_State := Game_Over;
      when Game_Over =>
         Noki.Log ("Game Over!");
         Game_State := Undefined;
      when others =>
         Noki.Log ("Press Ctrl+C to abort program!");
   end case;
   ```

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete `snake_game.adb` should look exactly like this:

```ada
with Noki;

procedure Snake_Game is
   type Game_State_T is (Welcome, Play, Game_Over, Undefined);
   Game_State : Game_State_T := Welcome;
begin
   loop
      case Game_State is
         when Welcome =>
            Noki.Log ("Welcome to Snakotron!");
            Game_State := Play;
         when Play =>
            Noki.Log ("We are playing.");
            Game_State := Game_Over;
         when Game_Over =>
            Noki.Log ("Game Over!");
            Game_State := Undefined;
         when others =>
            Noki.Log ("Press Ctrl+C to abort program!");
      end case;
      delay 1.0;
   end loop;
end Snake_Game;
```

**Build and run:**
```bash
alr build
bin/snake_game
```

❌ **If something goes wrong:** Check that your case statement syntax matches exactly, including the `when` clauses and semicolons.

✅ **Expected result:** 
```
Build finished successfully in ... seconds.
Welcome to Snakotron!
We are playing.
Game Over!
Press Ctrl+C to abort program!
Press Ctrl+C to abort program!
...
```

::remark-box
---
kind: warning
---
🤯 **Heads Up!** This program will run forever! Use `Ctrl+C` in the terminal to stop it.
::