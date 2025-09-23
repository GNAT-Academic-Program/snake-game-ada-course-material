---
kind: unit

title: Implement Your Custom Log Procedure

name: 4-snake
---

Let's make our Log procedure actually print to the terminal! We'll use Ada's standard I/O package for portable text output.

1. **Open the file** `src/noki.adb`

2. **Import the I/O package** at the very top of the file:
   ```ada
   with Ada.Text_IO;
   ```

3. **Replace the `null;` statement** inside the Log procedure with:
   ```ada
   Ada.Text_IO.Put (S);
   Ada.Text_IO.New_Line;
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

end Noki;
```

**Build the project:**
```bash
alr build
```

❌ **If something goes wrong:** Check that your syntax matches exactly, including semicolons and capitalization.

✅ **Expected result:** Successful build - your Log procedure now prints text to the terminal!
