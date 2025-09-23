---
kind: unit

title: Create a Game Engine Library in Ada

name: 2-snake
---

Let's create our first Ada library! We'll build a game engine called "noki" that will power our Snake game.

1. **Create the library** using Alire:
   ```bash
   alr -n init --lib noki
   ```

2. **Enter the noki directory:**
   ```bash
   cd noki
   ```

3. **Explore the generated structure**:
   ```bash
   ls -la
   ```

## What Alire Created

- **`alire.toml`**: Project configuration and dependencies
- **`src/`**: Source code directory  
- **`src/noki.ads`**: Library specification (public interface)

4. **Build the library** to verify everything works:
   ```bash
   alr build
   ```

::remark-box
---
kind: info
---
🤯 **Heads Up!** The `-n` flag tells Alire to skip interactive prompts and use defaults.
::

✅ **Expected result:** Alire creates a new library project structure with all necessary files, builds successfully, and you're now in the noki directory ready for development.

❌ **If the build fails:** Something went wrong with the setup. Start over from step 1 of lesson 2 to ensure everything is configured correctly.
