---
title: "A practical introduction to the GBVM: The Basics"
description: "An intro to the GBVM, a virtual machine for the Gameboy and Gameboy Color, used by GB Studio."
date: 2025-03-09
keywords: ["Gameboy", "GBVM", "GB Studio", "GBC","GB Studio"]
categories: ["Gameboy"]
tags: ["Gameboy", "GBVM", "GB Studio"]
series: ["An intro to the GBVM"]
series_order: 1

draft: true
---

{{< lead >}}
The GBVM seems daunting at first, but it's actually quite simple once you understand the basics.    
{{< /lead >}}

## What is the GBVM?

The GBVM is a virtual machine for the Gameboy and Gameboy Color, used by GB Studio. Every event you create in GB Studio is translated into GBVM instructions. You can make great games without ever needing to touch the GBVM, but it's useful to understand what's going on under the hood, so that we can harness its full power, if we need to.

## Why bother learning about it?

While you can make some amazing games without ever needing to touch the GBVM, it's useful to understand what's going on under the hood. There will often be situations in which you have to fight against GB Studio's event system, creating complex loops and conditional chains. In these cases, the GBVM is your friend. What may have taken hours to create in the event system can be done in just a few minutes with the GBVM.

## A real example

Imagine we want a vertical menu the full size of the screen. In GB Studio, we'd be limited to a horizontal menu at the bottom using <em>Display Menu</em>. With GBVM, we can have any kind of menu we like!

### Step 0: Create a new script
In GB Studio, let's create a new event on the <em>On Init</em> trigger of our scene. We can do this by clicking Add Event -> Joypad Input -> Attach Script to Button. We'll pick the Start button for our input. Then for the On Press, add a Miscellaneous -> GBVM Script. Nowe we can press the start button to trigger our script!

{{< figure src="p1-editor.png" alt="An open GBVM script editor" caption="An open GBVM script editor" >}}

### Step 1: Draw an overlay
An overlay, if you weren't familiar, is a frame that is drawn on top of the game. It can be used to draw things like menus, dialog boxes, and other UI elements. We can show, hide, and animate it around the screen.
```z80
VM_OVERLAY_SHOW 12, 0, .UI_COLOR_BLACK, .UI_DRAW_FRAME
```
GBVM instructions are written line by line, usually with a command <em>VM_OVERLAY_SHOW</em> followed by a list of arguments.
In the case of <em>VM_OVERLAY_SHOW</em>, the arguments are:

- How many tiles to the right to draw the frame.
- How many tiles down to draw the frame.
- The color of the frame.
- special flags.

So in the example above, we're drawing a frame 12 tiles to the right and 0 tiles down (So a long vertical frame at the right of the screen), in the color black, with the special flag <em>.UI_DRAW_FRAME</em>, which draws the frame selected in the UI Elements section of the GB Studio settings.

These can all be found on the [GBVM operations](https://www.gbstudio.dev/docs/scripting/gbvm/gbvm-operations/#vm_overlay_show) page of the GB Studio documentation.

{{< figure src="p1-game.png" alt="A vertical menu frame" caption="Our overlay is now visible on the screen!" >}}

### Step 2: Let's write some text
Now that we have our overlay visible, let's write some text on it.
```z80
VM_LOAD_TEXT 0
   .asciz "FIGHT\012ITEM\012RUN\012\012BACK"
VM_DISPLAY_TEXT
```
Now this one is a bit more complex, but we'll break it down.
```z80
VM_LOAD_TEXT 0
```
This is a command telling GBVM that we are about to give it some text, the 0 here is telling it that we don't want to give it any variables to fill in (We'll get to that later).
```z80
.asciz "FIGHT\012ITEM\012RUN\012\012\012BACK"
```
.asciz is a command that tells GBVM that we are giving it pure text, and that it should be treated as a string, not a command.
There are [special character codes](https://www.gbstudio.dev/docs/scripting/gbvm/gbvm-operations/#escape-sequences) that can be used to tell GBVM what to do with the text. In our case, \012 just means "write on a new line".
```z80
VM_DISPLAY_TEXT​
```
This command simply tells GBVM to display any text that is currently loaded. A nice and easy 3 step process! "Hey we're about to load some text, with no variables" -> "Here's the text" -> "Display it on the screen". Because we stated <em>.UI_DRAW_FRAME</em> when we made our overlay, the text will automatically be placed 1 tile down and 1 tile to the right, so it doesn't overlap with the frame.
{{< figure src="p2-game.png" alt="A vertical menu frame with text" >}}

### Step 3: Adjust our text
Our text is currently just to the right of the frame, but we actually want it to be 1 extra tile to the right, so we can have our little menu selection icon on the left column, so let's do that!
```z80
VM_LOAD_TEXT 1
   .asciz "\003\003\002FIGHT\012ITEM\012RUN\012\012\012BACK"
VM_DISPLAY_TEXT
```
As stated before, there are some special character codes that can be used to tell GBVM what to do with the text. In our case, \003 is telling GBVM that we are about to give it a tile offset for the text. \003\003\002 means "Offset 3 tiles to the right, and 2 tiles down" The automatic offset of 1 tile to the right and 1 tile down isn't included if we supply our own, so we're making up for it here!
Now our text is properly aligned!
{{< figure src="p3-game.png" alt="A vertical menu frame with text" >}}

### Step 4: Add our menu system!
Before we add our menu, we need to create a variable to store the choice. We can do this by renaming one of the 512 variables in the variable section of GB Studio. Let's rename variable 0 to <em>VAR_MENU_CHOICE</em>.
```z80
VM_CHOICE VAR_MENU_CHOICE, .UI_MENU_STANDARD 4
    .MENUITEM 1, 1, 0, 0, 0, 2
    .MENUITEM 1, 2, 0, 0, 1, 3
    .MENUITEM 1, 3, 0, 0, 2, 4
    .MENUITEM 1, 6, 0, 0, 3, 0
VM_OVERLAY_HIDE
```
This is getting a bit more complex, but let's break it down.
```z80
VM_CHOICE VAR_MENU_CHOICE, .UI_MENU_STANDARD 4
```
This is telling GBVM that we want to load a menu choice (i.e create our cursor and pause everything else). The <em>VAR_MENU_CHOICE</em> is the variable that will store the choice, and <em>.UI_MENU_STANDARD</em> is the style of menu we want to use. The 4 is telling GBVM that we want 4 options in our menu. There are of course [other menu options](https://www.gbstudio.dev/docs/scripting/gbvm/gbvm-operations/#vm_choice) that can be used, but this is the most basic one.
```z80
.MENUITEM 1, 1, 0, 0, 0, 2
```
- This is telling GBVM that we want to add an option to our menu. The first 2 numbers are the x,y tile positions of where our cursor should be, so in this example, 1 to the right, and 1 down. This is why we moved our text along in the previous step.
- The next 4 numbers are to do with menu navigation. We've said we have 4 options, so we need to tell GBVM where to go when the player presses any of the 4 directional buttons. The first number is what option we change to when the player presses the left button, the second is what option we change to when the player presses the right button, the third is what option we change to when the player presses the up button, and the fourth is what option we change to when the player presses the down button.
- In our example, the first 2 digits <em>0,0</em> tell GBVM that we don't want to change our option when the player presses the left or right buttons. We're making a vertical menu after all!
- The next 0 tells GBVM that we don't want to change our option when the player presses the up button. We're at the top of the list, so it makes sense! If we wanted this menu to wrap around, we could put the number 4 here, to make it go to the bottom of the list.
- The last number <em>2</em> is what happens when the player selects the down button. In our case, we want to move onto the next option in the list, which would be number 2.
```z80
    .MENUITEM 1, 2, 0, 0, 1, 3
    .MENUITEM 1, 3, 0, 0, 2, 4
    .MENUITEM 1, 6, 0, 0, 3, 0
```
- Hopefully you can make sense of the rest of the options! The first 2 are directly below the first, so we still offset 1 to the right, but now 2 and 3 tiles down. The last line of text we wrote <em>"BACK"</em> had 3 new lines in it <em>\012\012\012</em>, so it's offset 6 tiles down, instead of 4.
- Then we see that pressing up should select the higher option (e.g pressing up on option 4 selects option 3), and pressing down should select the lower option (e.g pressing down on option 4 does nothing, so we have a 0.)
```z80
VM_OVERLAY_HIDE
```
- Whenever GBVM sees a VM_CHOICE command, it will pause the game, and wait for the player to make a choice. When the player makes a choice, GBVM will store the choice number in the <em>VAR_MENU_CHOICE</em> variable we have selected. In our case, if the player selects option 1, <em>VAR_MENU_CHOICE</em> will be set to 1. If the player selects option 2, <em>VAR_MENU_CHOICE</em> will be set to 2, and so on.
- The <em>VM_OVERLAY_HIDE</em> command simply hides the overlay, so we can go back to the game. The reason this doesn't instantly happen is because of the pausing effect of the VM_CHOICE command previously mentioned.
{{< figure src="p4-game.png" alt="A vertical menu frame with text" caption="Our menu is now navigable!">}}

### Step 5: Let's reflect on what we've done

We know how to create an overlay on the screen of any size and position. We know how to write text to the overlay, and we know how to create a menu and store the player's choice.
Just with these simple few steps, we know enough to create a vast array of menus and UI elements, from selection menus, to status screens, to dialog boxes! This is the power of using GBVM over the preset GB Studio event system. We won't always need to use GBVM, but it's good to know that if a built-in event doesn't do what we need, we can use GBVM to get the job done.

### Step 6: Let's make things happen!
We've got a menu, and we've got a choice, but we don't have any way to handle the choice, so let's do that now. Let's make it so that if the player selects "Fight" that we'll show some text, and if they select "Item" that we'll track how many times they've pressed the item option.

```z80
VM_OVERLAY_SHOW 0, 16, .UI_COLOR_BLACK, .UI_DRAW_FRAME
VM_IF VAR_MENU_CHOICE, .EQ, 1
    VM_LOAD_TEXT 0
    .asciz "You swing your sword at the enemy!"
    VM_DISPLAY_TEXT
VM_IF VAR_MENU_CHOICE, .EQ, 2
    VM_LOAD_TEXT 0
    .asciz "You use an invisbility potion!"
    VM_DISPLAY_TEXT
    VM_HIDE_SPRITES

VM_OVERLAY_HIDE
```
{{< figure src="p6-game.png" alt="A vertical menu frame with text" >}}








































////
```
