---
kind: unit

title: Convert Pixels to ANSI Strings

name: 2-snake
---

Let's build a pixel rendering system! We need to convert our pixel model into ANSI escape codes that the terminal can display.

## Understanding Terminal as a 2D Canvas

Our rendering target is a modern terminal emulator. With ANSI escape codes we can move the cursor and apply styles, so **the terminal's 2D space acts like a pixel buffer** - a rendered image.

Instead of scattering escape codes everywhere, we'll build semantic helpers to draw pixels.

## 1. Add Wide_Wide_String Support

Edit `noki.ads` and add this after the `WWChar_T` subtype:

```ada
subtype WWStr_T is Wide_Wide_String;
```

**Why?** In Ada, a String is an array of Character. We need the Wide_Wide (Unicode) version for our pixel rendering.

## 2. Create the Pixel-to-String Converter

We'll overload the `+` operator for clean, dense syntax. Add this signature to `noki.ads`:

```ada
function "+" (P : Pixel_T) return WWStr_T;
```

## 3. Start the Implementation

Edit `noki.adb` and add the function stub:

```ada
function "+" (P : Pixel_T) return WWStr_T is
begin
   null;
end "+";
```

## 4. Add the Trim Helper Function

**Key insight:** When Ada converts numbers to strings, it systematically adds a space for the sign:
- `-1` becomes `"-1"` (2 characters: `['-', '1']`)  
- `1` becomes `" 1"` (2 characters: `[' ', '1']`), the space is for the implicit `+`

We need to trim this leading space. Add this helper function before the `+` function:

```ada
function Trim (S : String) return String is
   (S (S'First + 1 .. S'Last));
```

**Ada features used:**
- Function expressions (skip `begin/end` for single expressions)
- Array attributes: `'First` and `'Last` give array bounds
- Range notation: `..` for slicing arrays

## 5. Build the Color Control Strings Step by Step

**Remember:** We already defined the `CSI` constant at the top of `noki.ads`.

Now let's build the ANSI control strings inside the `+` function. Add these constants between `is` and `begin`:

**First, add the foreground color:**
```ada
FG_Color : constant String := CSI & "38;2;" & 
                              Trim (P.FC.R'Image) & ";" & 
                              Trim (P.FC.G'Image) & ";" & 
                              Trim (P.FC.B'Image) & "m";
```

For RGB values like R=44, G=192, B=255, this creates: `"\e[38;2;44;192;255m"`

::remark-box
---
kind: info
---
🤯 **Heads Up!** The ESC character (byte 27 or hex 16#1B#) has no glyph - that's why you see notations like `\e` or `^[` in documentation. These are just human-readable representations of the actual escape character.
::

**Next, add the background color** (same pattern, different code):
```ada
BG_Color : constant String := CSI & "48;2;" & 
                              Trim (P.BC.R'Image) & ";" & 
                              Trim (P.BC.G'Image) & ";" & 
                              Trim (P.BC.B'Image) & "m";
```

**Add the bold control:**
```ada
BOLD : constant String := CSI & "1m";
```

**Add the reset control:**
```ada
RESET : constant String := CSI & "0m";
```

**Combine our pixel format conditionally:**
```ada
FORMAT : constant String := (if P.B then 
                                FG_Color & BG_Color & BOLD 
                             else 
                                FG_Color & BG_Color);
```

## 6. Complete the Function

Now we're ready to implement the function body using our building blocks. 

**Understanding the type conversion:** Our function returns `WWStr_T` (Unicode string), and our pixel character `P.C` is already `WWChar_T` (Unicode). However, our ANSI control strings (`FORMAT`, `RESET`) are regular `String` type, so we need explicit conversion to satisfy Ada's strong typing.

**Add the conversion package** at the top of `noki.adb`:
```ada
with Ada.Characters.Conversions;
```

**Complete the function** by replacing the `null;` line with:
```ada
return Ada.Characters.Conversions.To_Wide_Wide_String (FORMAT) & 
       P.C & 
       Ada.Characters.Conversions.To_Wide_Wide_String (RESET);
```

**Key insight:** We convert the String control codes to Wide_Wide_String, then concatenate with the Unicode character to create the complete formatted pixel string.

## 💡 **Bonus: Function Renaming**

As you can see, `Ada.Characters.Conversions.To_Wide_Wide_String` is verbose to write repeatedly. Ada allows you to rename functions for convenience!

**Add this function rename** inside the `+` function (in the declarative section):
```ada
function WWS (S : String) return WWStr_T renames Ada.Characters.Conversions.To_Wide_Wide_String;
```

**Then simplify your return statement** to:
```ada
return WWS (FORMAT) & P.C & WWS (RESET);
```

**Why this works:** Function renames create an alias with the same signature, making your code cleaner while maintaining full type safety.

## 📋 **Code Review - Complete `+` Function**

Your complete implementation should look like this:

```ada
function Trim (S : String) return String is
   (S (S'First + 1 .. S'Last));

function "+" (P : Pixel_T) return WWStr_T is
   BOLD     : constant String := CSI & "1m";
   RESET    : constant String := CSI & "0m";
   FG_Color : constant String := CSI & "38;2;" & 
                                 Trim (P.FC.R'Image) & ";" & 
                                 Trim (P.FC.G'Image) & ";" & 
                                 Trim (P.FC.B'Image) & "m"; 
   BG_Color : constant String := CSI & "48;2;" & 
                                 Trim (P.BC.R'Image) & ";" & 
                                 Trim (P.BC.G'Image) & ";" & 
                                 Trim (P.BC.B'Image) & "m";
   FORMAT   : constant String := (if P.B then 
                                     FG_Color & BG_Color & BOLD 
                                  else 
                                     FG_Color & BG_Color);
   function WWS (S : String) return WWStr_T 
      renames Ada.Characters.Conversions.To_Wide_Wide_String;
begin
   return WWS (FORMAT) & P.C & WWS (RESET);
end "+";
```


✅ **Expected result:** You can now convert any `Pixel_T` to a formatted terminal unicode string using the `+` operator!
