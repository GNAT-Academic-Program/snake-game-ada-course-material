---
kind: unit

title: We Need Pixels

name: 2-snake
---

Let's create pixel abstractions for our terminal game! Our "pixel" combines foreground color, background color, character, and boldness - different from screen pixels but perfect for terminal graphics.

## Understanding Ada's Type System

Before we build pixels, let's understand Ada's powerful type system:

**Modular Types:** `type U8_T is mod 2 ** 8` creates a type that wraps around (0-255). When it overflows, it loops back to 0. Perfect for hardware registers and color values.

**Records:** Like structs in C - group related data together with named fields.

**Subtypes:** `subtype Bold_T is Boolean` creates a compatible alias. You can use `Bold_T` anywhere you'd use `Boolean` without conversion.

**Strong Typing:** Ada prevents mixing incompatible types. You can't accidentally add `Miles_T` and `Kilometers_T` - you need explicit conversion. This catches bugs at compile time.

1. **Define basic types** in `noki.ads` after `package Noki is`:
   ```ada
   subtype WWChar_T is Wide_Wide_Character;  -- Unicode characters (less verbose)
   
   type U8_T is mod 2 ** 8 with Size => 8;   -- 0-255, wraps on overflow
   
   type Color_T is record                    -- RGB color structure
      R, G, B : U8_T;
   end record with Size => 32;
   
   subtype Bold_T is Boolean;                -- Semantic clarity over raw Boolean
   ```

2. **Create the Pixel record**:
   ```ada
   type Pixel_T is record
      FC : Color_T;         -- Foreground Color
      BC : Color_T;         -- Background Color
      C  : WWChar_T;        -- Character
      B  : Bold_T := False; -- Bold (default False)
   end record;
   ```

3. **Add colors for our game**:
   ```ada
   Deep_Navy     : Color_T := (13, 2, 33);
   Hot_Tangerine : Color_T := (255, 122, 24);
   Acid_Lime     : Color_T := (201, 255, 0);
   Neon_Pink     : Color_T := (255, 102, 209);
   ```

## Organizing Game-Specific Code

The previous definitions are game-agnostic and belong in our library `noki.ads`. Now let's create things specific to our snake game.

**Why separate packages?** We could put everything in our main binary, but that rapidly clutters the code. A better approach is creating a specific package for our game to keep things organized and comprehensible.

4. **Create a Snake package** - make a new file `snake.ads` next to `snake_game.adb`:
   ```ada
   with Noki; use Noki;
   package Snake is

   end Snake;
   ```

Inside `snake.ads` add the following:

5. **Define snake characters** (showing different Unicode initializations):
   ```ada
   Tongue : constant WWChar_T := '~';                     -- ASCII literal
   Head   : constant WWChar_T := WWChar_T'Val (16#25C0#); -- Unicode by hex value
   Torso  : constant WWChar_T := '◆';                     -- Direct Unicode paste
   ```

6. **Create actual snake pixels**:
   ```ada
   Tongue_Pix : Pixel_T := (FC => Hot_Tangerine, 
                            BC => Deep_Navy, 
                            C  => Tongue, 
                            B  => True);  
   Head_Pix   : Pixel_T := (Acid_Lime, Deep_Navy, Head, True);
   Torso_Pix  : Pixel_T := (Neon_Pink, Deep_Navy, Torso, True);
   ```

## 📋 **Code Review - Complete Files**

Your `noki.ads` should include these game-agnostic types after `package Noki is`:

```ada
subtype WWChar_T is Wide_Wide_Character;

type U8_T is mod 2 ** 8 with Size => 8;

type Color_T is record
   R, G, B : U8_T;
end record with Size => 32;

subtype Bold_T is Boolean;

type Pixel_T is record
   FC : Color_T;         -- Foreground Color
   BC : Color_T;         -- Background Color
   C  : WWChar_T;        -- Character
   B  : Bold_T := False; -- Bold
end record;

Deep_Navy     : Color_T := (13, 2, 33);
Hot_Tangerine : Color_T := (255, 122, 24);
Acid_Lime     : Color_T := (201, 255, 0);
Neon_Pink     : Color_T := (255, 102, 209);
```

Your `snake.ads` should contain the snake-specific definitions:

```ada
with Noki; use Noki;
package Snake is

   Tongue : constant WWChar_T := '~';
   Head   : constant WWChar_T := WWChar_T'Val (16#25C0#);
   Torso  : constant WWChar_T := '◆';

   Tongue_Pix : Pixel_T := (FC => Hot_Tangerine, 
                            BC => Deep_Navy, 
                            C  => Tongue, 
                            B  => True);
   Head_Pix   : Pixel_T := (Acid_Lime, Deep_Navy, Head, True);
   Torso_Pix  : Pixel_T := (Neon_Pink, Deep_Navy, Torso, True);

end Snake;
```

✅ **Expected result:** Clean separation between game library and snake-specific code!