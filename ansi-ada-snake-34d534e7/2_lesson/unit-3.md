---
kind: unit

title: Expose a Custom Log Method in Your Noki Library

name: 3-snake
---

Let's create our first Ada procedure! We'll add a `Log` method to our noki library for printing messages.

1. **Open the specification file** `src/noki.ads`:
   - Use the **file explorer** on the left side of the IDE
   - Navigate to `noki` → `src` → `noki.ads` and click to open it
   - The extension `.ads` means **Ada Specification**
   - This file is the **public interface** of your compilation unit
   - It's like `.h/.hpp` in C/C++, but Ada uses **semantic modules** (called *packages*) instead of text pre-processing

2. **Add the procedure declaration** right after the line `package Noki is`:
   ```ada
   procedure Log (S : String);
   ```

3. **Create the implementation file** `src/noki.adb`:
   - `.adb` means Ada Body
   - This is similar to a `.c/.cpp` file in C/C++
   - Using the IDE menu create a new file `src/noki.adb`

4. **Add the package body structure** to `src/noki.adb`:
   ```ada
   package body Noki is

   end Noki;
   ```

5. **Implement the Log procedure** inside the package body:
   ```ada
   procedure Log (S : String) is
   begin
      null;
   end Log;
   ```

## 📋 **Code Review - Complete `noki.adb`**

Your complete `noki.adb` should look like this:

```ada
package body Noki is

   procedure Log (S : String) is
   begin
      null;
   end Log;

end Noki;
```

**Build the library:**
```bash
alr build
```

❌ **If something goes wrong:** If you don't see `Build finished successfully in ...`, re-check the instructions carefully and start again from step 1.

✅ **Expected result:** Successful build with no compilation errors.
