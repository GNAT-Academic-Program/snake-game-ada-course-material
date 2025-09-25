---
kind: unit

title: Control the Game Rendering Using ANSI Escape Codes

name: 3-snake
---

Let's control our terminal rendering! Right now our output just appends to the terminal. For a proper game, we need to update what's displayed instead.

**Quick ANSI background:** ESC (ASCII 27, hex 0x1B) + `[` = Control Sequence Introducer (CSI). This tells the terminal "what follows is a command, not text to print."

1. **Create a reusable CSI constant** in `noki.ads`:
   - Before the `procedure Log (S : String);` line, add:
   ```ada
   CSI : constant String := Character'Val (16#1B#) & '[';
   ```

2. **Add a screen clearing function** right after the CSI line:
   ```ada
   function Clear_Screen return String is (CSI & "2J" & CSI & "H");
   ```
   
   This builds the ANSI command: clear screen (`2J`) + move cursor to home (`H`).

3. **Use it in our game loop** by opening `snake_game.adb` and adding this right after `loop`:
   ```ada
   Noki.Log (Noki.Clear_Screen);
   ```

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete `snake_game.adb` should look like this:

```ada
with Noki;

procedure Snake_Game is
   type Game_State_T is (Welcome, Play, Game_Over, Undefined);
   Game_State : Game_State_T := Welcome;
begin
   loop
      Noki.Log (Noki.Clear_Screen);
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
alr run
```

✅ **Expected result:** Instead of scrolling text, you'll see only the current message updating in place as the game state advances.

::remark-box
---
kind: info
---
🤯 **Heads Up!** The terminal now clears and redraws each frame - this is the foundation of all terminal-based games!
::