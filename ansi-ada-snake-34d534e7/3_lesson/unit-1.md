---
kind: unit

title: Add the Loop

name: 1-snake
---

Let's create a game loop! This will handle our different gameplay states and keep the game running.

1. **Edit `snake_game.adb`** to add a loop around the existing code

2. **Add loop structure** around the `Noki.Log` call:
   ```ada
   loop
      Noki.Log ("Welcome to my Snake Game!");
   end loop;
   ```

3. **Add a delay** to slow down the loop so we can see the output:
   - On the line after `Noki.Log ("Welcome to my Snake Game!")` add:
   ```ada
   delay 1.0;
   ```

**Your complete code inside the `Snake_Game` procedure should look like this:**
```ada
loop
   Noki.Log ("Welcome to my Snake Game!");
   delay 1.0;
end loop;
```

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete file should look like this:

```ada
with Noki;

procedure Snake_Game is
begin
   loop
      Noki.Log ("Welcome to my Snake Game!");
      delay 1.0;
   end loop;
end Snake_Game;
```

**Build and run:**
```bash
alr run
```

❌ **If something goes wrong:** If you don't see `Build finished successfully in ...`, re-check the instructions carefully and start again from step 1.

✅ **Expected result:** `Welcome to my Snake Game!` appearing every second over and over.

::remark-box
---
kind: warning
---
🤯 **Heads Up!** This program will run forever! Use `Ctrl+C` in the terminal to stop it.
::