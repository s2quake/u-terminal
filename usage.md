---
layout: page
permalink: /usage
permalink_name: /usage
title: Usage
---

# Table of contents

[1.Structure](#structure)
[2.Terminal creation](#terminal-creation)
[3.Mutiple Terminal creation](#mutiple-terminal-creation)
[4.Change style](#change-style)
[5.Slide terminal](#slide-terminal)
[6.Change dock](#change-dock)

***

## /Structure

<div><br></div>
<div><span>Canvas</span></div>
<div><span>┣TerminalRoot</span><span style="float: right;">[TerminalSlidingController]</span></div>
<div><span>┃ ┣TerminalLayout</span><span style="float: right;">[TerminalDockController]</span></div>
<div><span>┃ ┃ ┗Terminal</span><span style="float: right;">[Terminal, TerminalGrid, CommandContextHost]</span></div>
<div><span>┃ ┃ &nbsp;&nbsp;┣TerminalBackground</span></div>
<div><span>┃ ┃ &nbsp;&nbsp;┃TerminalCursor</span></div>
<div><span>┃ ┃ &nbsp;&nbsp;┃TerminalForeground</span></div>
<div><span>┃ ┃ &nbsp;&nbsp;┃TerminalComposition</span></div>
<div><span>┃ ┃ &nbsp;&nbsp;┗TerminalScrollbar</span></div>
<div><span>┃ ┗ KeyboardLayout</span></div>
<div><span>┣TerminalDispatcher</span></div>
<div><span>┗TerminalStyles</span></div>



## /Terminal creation

Execute the menu below in the hierarchy window.

```
GameObject -> UI -> Terminals -> Terminal Full - Commands
```

<div class="video-container">
    <iframe width="100%" src="https://www.youtube.com/embed/WU2DbKSWV_M" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

### Creation menus

* GameObject -> UI -> Terminals -> Terminal
  * Create only pure terminal that exclude commands and default layouts.
* GameObject -> UI -> Terminals -> Terminal - Commands
  * Create terminals with excluded default layouts.
* GameObject -> UI -> Terminals -> TerminalLayout
  * Create only the default layout without a terminal.
* GameObject -> UI -> Terminals -> Terminal Full
  * Create a terminal that contains a default layout except for the command.
* GameObject -> UI -> Terminals -> Terminal Full - Commands
  * Create a terminal that contains commands and default layouts.

***

## /Mutiple terminal creation

<div class="video-container">
    <iframe width="100%" src="https://www.youtube.com/embed/lY5-wWMmik4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

***

## /Change style

Change style in the 'TerminalGrid' component of a 'Terminal' GameObject.

or

Change style using the 'style' command in the terminal during play

<div class="video-container">
    <iframe width="100%" src="https://www.youtube.com/embed/FqxaGUOnPYQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

***

## /Slide terminal

You can change the settings in the 'TerminalSlidingController' component of the 'TerminalRoot' GameObject.

<div class="video-container">
    <iframe width="100%" src="https://www.youtube.com/embed/3z1feX2_Wy0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

***

## /Change dock

Change dock in the 'TerminalDockController' component of a 'TerminalLayout' GameObject.

or

Change dock using the 'terminal dock' command in the terminal during play

<div class="video-container">
    <iframe width="100%" src="https://www.youtube.com/embed/0xqb9nAXZTo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
