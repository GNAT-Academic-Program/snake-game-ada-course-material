---
kind: unit

title: Create an Input Task

name: 3-snake
---

Let's create a thread to handle keyboard input! This will run concurrently with our game loop, allowing real-time user interaction.

1. **Declare a task type** in `noki.ads` before `end Noki;`:
   ```ada
   task type Input_Task_T;
   ```

2. **Implement the task body** in `noki.adb` before `end Noki;`:
   ```ada
   task body Input_Task_T is
      C : Character := 'X';
   begin
      loop
         Ada.Text_IO.Get_Immediate (C); -- blocking
         Log ("Got a character: " & C'Image);
      end loop;
   end Input_Task_T;
   ```

::remark-box
---
kind: info
---
🤯 **Heads Up!** The task loops indefinitely, blocking until a character is available, then puts that char into C and loops again waiting for a new character. It logs the character using `C'Image` (Ada 2022 feature for String representation of any type).
::

3. **Use the input task** in `snake_game.adb`:
   - Add a task variable in the declarative section (the task starts looping immediately):
   ```ada
   Input_Task : N.Input_Task_T;
   ```

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete `snake_game.adb` should now look like this:

```ada
with Noki;

procedure Snake_Game is
   package N renames Noki;
   type Game_State_T is (Welcome, Play, Game_Over, Undefined);
   Game_State : Game_State_T := Welcome;
   Input_Task : N.Input_Task_T;
begin
   loop
      N.Log (N.Clear_Screen);
      case Game_State is
         when Welcome =>
            N.Log ("Welcome to Snakotron!");
            Game_State := Play;
         when Play =>
            N.Log ("We are playing.");
            Game_State := Game_Over;
         when Game_Over =>
            N.Log ("Game Over!");
            Game_State := Undefined;
         when others =>
            N.Log ("Press Ctrl+C to abort program!");
      end case;
      delay 1.0;
   end loop;
end Snake_Game;
```

✅ **Expected result:** Your game now handles keyboard input concurrently! Press keys while the game runs to see "Got a character: ..." messages.
