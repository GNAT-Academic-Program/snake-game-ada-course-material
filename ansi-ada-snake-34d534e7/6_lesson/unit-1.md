---
kind: unit

title: Building Texture Types with Ada Vectors

name: 1-snake
---

Let's build a proper texture system! Instead of manually calling `Draw (Torso_Pix)` four times, we'll create 2D textures using Ada's generic vector containers.

## Understanding Ada Generics and Vectors

**Ada Generics** are like C++ templates but require explicit instantiation. You can't just use `Vector<Pixel>` - you must create a specific package instance first.

**Why Vectors?** Ada's `Ada.Containers.Vectors` provides dynamic arrays that can grow and shrink. Perfect for storing rows and columns of pixels.

1. **Import the Vector package** - add to the top of `noki.ads`:
   ```ada
   with Ada.Containers.Vectors;
   ```

## Building a 1D Pixel Vector

2. **Create our first vector package** - add after your existing types in `noki.ads`:
   ```ada
   package Pixels_N_Vecs is new
     Ada.Containers.Vectors
       (Index_Type   => Natural,
        Element_Type => Pixel_T);
   ```

**Why Natural?** `Natural` is defined as `0 .. Integer'Last` - perfect for array indexing starting from 0.

3. **Add the use clause** to bring operations into scope:
   ```ada
   use Pixels_N_Vecs;
   ```

**What does `use` do?** Without it, you'd need to write `Pixels_N_Vecs.Append`, `Pixels_N_Vecs.Length`, etc. The `use` clause lets you write just `Append`, `Length` directly.

4. **Create a convenient subtype** to reduce verbosity:
   ```ada
   subtype Pixels_N_T is Pixels_N_Vecs.Vector;
   ```

**Package vs Type:** `Pixels_N_Vecs` is the package containing operations. `Vector` is the actual type inside that package. Our subtype `Pixels_N_T` is just a shorter name for `Pixels_N_Vecs.Vector`.

## Building a 2D Texture (Vector of Vectors)

5. **Create the 2D vector package** - each element is now a row of pixels:
   ```ada
   package Pixels_NxM_Vecs is new
     Ada.Containers.Vectors
       (Index_Type   => Natural,
        Element_Type => Pixels_N_T);
   use Pixels_NxM_Vecs;
   subtype Pixels_NxM_T is Pixels_NxM_Vecs.Vector;
   ```

6. **Create our final texture type**:
   ```ada
   subtype Texture_T is Pixels_NxM_T;
   ```

**Why another subtype?** `Texture_T` gives semantic meaning - when you see it in code, you know it's a 2D graphics texture, not just any vector.

7. **Add an empty pixel** for transparent areas:
   ```ada
   Black     : Color_T := (0, 0, 0);
   Empty_Pix : Pixel_T := (Black, Black, ' ', False);
   ```

## Creating the Render System

8. **Add render procedure spec** to `noki.ads`:
   ```ada
   procedure Render (T : Texture_T);
   ```

9. **Implement the renderer** in `noki.adb` after your `Draw` procedure:
   ```ada
   procedure Render (T : Texture_T) is
      function Move_Cursor (Row : Positive; Col : Positive) return String is
        (CSI & Trim (Row'Image) & ";" & Trim (Col'Image) & "H");
   begin
      for Y in T.First_Index .. T.Last_Index loop
         Ada.Text_IO.Put (Move_Cursor (Y + 2, Positive'First));
         for X in T (Y).First_Index .. T (Y).Last_Index loop
            Draw (T (Y) (X));
         end loop;
      end loop;
   end Render;
   ```

## Understanding Local Functions

**Why is `Move_Cursor` local?** We declare it inside `Render` because:
- **Scope:** Only `Render` needs cursor movement - no other procedure uses it
- **Encapsulation:** Keeps the function close to where it's used
- **Namespace:** Doesn't pollute the global namespace with implementation details

**Local Declaration Syntax:** In Ada, you declare local functions between `is` and `begin`. They can access the outer procedure's parameters (like `T` in our case).

**Terminal Coordinates:** Terminals use 1-based indexing, not 0-based. That's why we use `Positive` type (1 .. Integer'Last) - it prevents accidental zero coordinates that would crash the program.

## Testing Our Texture System

10. **Create a player texture** in `snake_game.adb` declarative section:
    ```ada
    Player : Texture_T := [[Tongue_Pix, Head_Pix,  Empty_Pix],
                           [Empty_Pix,  Torso_Pix, Empty_Pix],
                           [Empty_Pix,  Torso_Pix, Torso_Pix]];
    ```

11. **Update the Play state** to use our new renderer:
    ```ada
    when Play =>
       Render (Player);
       Game_State := Game_Over;
    ```

## 📋 **Code Review - Complete Addition to noki.ads**

Add this after your existing types:

```ada
package Pixels_N_Vecs is new
  Ada.Containers.Vectors
    (Index_Type   => Natural,
     Element_Type => Pixel_T);
use Pixels_N_Vecs;
subtype Pixels_N_T is Pixels_N_Vecs.Vector;

package Pixels_NxM_Vecs is new
  Ada.Containers.Vectors
    (Index_Type   => Natural,
     Element_Type => Pixels_N_T);
use Pixels_NxM_Vecs;
subtype Pixels_NxM_T is Pixels_NxM_Vecs.Vector;
subtype Texture_T is Pixels_NxM_T;

Black     : Color_T := (0, 0, 0);
Empty_Pix : Pixel_T := (Black, Black, ' ', False);

procedure Render (T : Texture_T);
```

::remark-box
---
kind: info
---
🤯 **Heads Up!** The double bracket syntax `[[...], [...]]` creates a 2D array literal. Each inner bracket is one row of pixels.
::

✅ **Expected result:** Your snake now renders as a proper 2D texture with clean, organized code!
