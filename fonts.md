---
layout: page
permalink: /fonts
permalink_name: /fonts
title: Fonts
---

# Font

## 1. Download font

* Select one of the [Google Fonts](https://fonts.google.com/?category=Monospace) with Monospace type.

## 2. Download font-generator

* https://github.com/s2quake/font-generator/releases

## 3. Export Font Data and Textures

```console
dotnet jsfont.dll <font-path> <output-path>
```

> You can find out more about using 'dotnet jsfont.dll' [here](https://github.com/s2quake/font-generator/releases).

## 4. Copy To Assets Path

* copy fnt file and png texture files to unity assets path fnt.

## 5. Create Font Descriptor

* In the Unity project window, select the fnt file.
* Select **Assets - Create - Terminal - Create Font Descriptor** from the top menu to create an object containing font information from the fnt file.

## 6. Create Font

* Select **Assets - Create - Terminal - Font** from the top menu to create an empty font object.
* Select a font object and set the size value of the **Descriptor List** to 1 in the inspector window.
* Set the **Font Descriptor** generated in the previous step in the **Element 0** property.

## 7. Apply Font

* Select the Terminal object (Canvas/TerminalRoot/TerminalLayout/Terminal) in the Hierarchy window and set the newly created font object in the Terminal component in the Inspection window.

# jsfont.dll Usage

## Usage

```console
dotnet jsfont.dll <font-path> <output-path>
```

## Requirements

\<font-path\>

```
Indicates the font path.
```

\<output-path\>

```
Indicates the output path.
If no output path is specified, the font information is output.

If the output path is a directory, the file is automatically named based on the font name.
```

## Options

\-\-characters 'value'

```
Indicates the Unicode of the character you want to export.
expressed in decimal or hexadecimal form, each character is separated by ','
You can use '-' to specify multiple characters.
e.g. "65, 0x41, 66-90, 0x61-0x7A"

If no value is specified, export all characters.
```

\-\-dpi 'value'

```
Indicates the DPI of the font. The default value is 72.
```

\-\-size 'value'

```
Indicates the size of the font. The default value is 26.
```

\-\-face 'value'

```
Indicates the type of font. The default value is 0.
```

\-\-texture-width 'value'

```
Indicates the width of the texture to be printed. The default value is 512.
```

\-\-texture-height 'value'

```
Indicates the height of the texture to be output. The default value is 512.
```

\-\-padding 'value'

```
Indicates the padding of the character. 
You can set the value for both sides. The default value is 1.
    e.g. 1) --padding 1 sets all padding to 1.
    e.g. 2) --padding "1, 2" sets the left and right padding to 1 and the top and bottom padding to 2.
    e.g. 3) --padding "1 1 0 0" sets left to 1, top to right to 0 and bottom to 0.
```

\-\-spacing 'value'

```
Indicates the gap between the character and characters.
You can set horizontal and vertical values. The default value is 1.
    e.g. 1) --spacing 1 sets the horizontal and vertical spacing to 1.
    e.g. 2) --spacing "1 0" sets only the horizontal interval to 1.
```
