---
kind: unit

title: Making Things Less Verbose

name: 2-snake
---

Let's reduce verbosity! Ada lets you control how explicit you want to be with package names. Instead of typing `Noki.` everywhere, we can create shorter aliases.

1. **Add a package rename** in `snake_game.adb`:
   - Just under `procedure Snake_Game is`, add:
   ```ada
   package N renames Noki;
   ```

2. **Replace all `Noki.` with `N.`** throughout your code:
   - `Noki.Log (Noki.Clear_Screen);` becomes `N.Log (N.Clear_Screen);`
   - Update all other `Noki.Log` calls to `N.Log`

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete `snake_game.adb` should look like this:

```ada
with Noki;

procedure Snake_Game is
   package N renames Noki;
   type Game_State_T is (Welcome, Play, Game_Over, Undefined);
   Game_State : Game_State_T := Welcome;
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

✅ **Expected result:** Same behavior, less typing!

::remark-box
---
kind: info
---
🤯 **Heads Up!** In large codebases, explicit package names (`Noki.Log`) are often more readable than short aliases (`N.Log`) or dropping the namespace entirely (`Log`). They show exactly where code comes from. Balance brevity with clarity based on your project's needs.
::
