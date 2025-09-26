---
kind: unit

title: Use Your Custom Log Procedure

name: 5-snake
---

Let's test our Log procedure! We'll create an executable program that uses our noki library.

1. **Verify that `Log` is exported** by running this in the terminal:
   ```bash
   nm lib/libNoki.a
   ```
   
   ✅ **Expected result:** You should see something like:
   ```
   noki.o:
   ...
   0000000000000000 T noki__log
   ```

2. **Create a binary project** to use the noki library:
   - Make sure you're in the `noki` folder:
     ```bash
     cd ~/sandbox/snake-tutorial/noki
     ```
   - Create a new executable project named `snake_game`:
     ```bash
     alr -n init --bin snake_game
     ```

3. **Make your executable depend on your library:**
   - Move into the new project:
     ```bash
     cd snake_game
     ```
   - Open the `snake-tutorial` → `noki` → `snake_game` → `alire.toml` file and add this at the end of the file:
     ```toml
     [[depends-on]]
     noki = "*"

     [[pins]]
     noki = { path = ".." }

     [environment]
     ADAFLAGS.append = "-gnat2022"
     ```

4. **Call `Noki.Log` in your program:**
   - Open the `snake-tutorial` → `noki` → `snake_game` → `src` → `snake_game.adb`
   - At the top, add the package from your library:
     ```ada
     with Noki;
     ```
   - Replace `null;` with:
     ```ada
     Noki.Log ("Welcome to my Snake Game!");
     ```

## 📋 **Code Review - Complete `snake_game.adb`**

Your complete `snake_game.adb` should look like this:

```ada
with Noki;

procedure Snake_Game is
begin
   Noki.Log ("Welcome to my Snake Game!");
end Snake_Game;
```

**Build and run:**
```bash
alr build
bin/snake_game
```

❌ **If something goes wrong:** If you don't see `Build finished successfully in ...`, re-check the instructions carefully and start again from step 1.

✅ **Expected result:** On the terminal you should see: `Welcome to my Snake Game!`


::remark-box
---
kind: info
---
🤯 **Heads Up!** This tells Alire to use the local `noki` library from the parent folder.
::
