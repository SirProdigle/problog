---
title: "PROJECT: Gameboy Image Converter"
description: "A Gradio-based tool for converting images into authentic Game Boy & Game Boy Colour graphics"
date: 2025-03-08T00:15:06.000Z
showReadingTime: false
showDate: false
showWordCount: false
keywords: ["Gameboy", "Image Converter", "Gradio", "Python", "NumPy", "scikit-image", "scikit-learn", "SciPy"]
categories: ["Gameboy"]
tags: ["Gameboy", "GB Studio", "Python"]
---

{{< lead >}}
✨ A user-friendly tool that transforms images into authentic Game Boy & Game Boy Color graphics.
{{< /lead >}}


## 🔍 What is it?

The Gameboy Image Converter is a simple web tool that takes your modern images and converts them into Game Boy and Game Boy Color hardware-compatible graphics, with a focus on GB Studio developers.

{{< alert "wand-magic-sparkles" >}}
Try it yourself at [gameboy.prodigle.dev](https://gameboy.prodigle.dev)
{{< /alert >}}

## 🖼️ See It In Action
{{< figure
    src="spyggt.webp"
    alt="Original image"
    caption="Original artwork (by [Spyggt](https://www.x.com/spyggt))"
  >}}
  {{< figure
      src="ho-oh-gb-palette.png"
      alt="Original image with GB palette"
      caption="Game Boy Palette"
      class="max-w-full lg:max-w-xs"
  >}}
  {{< figure
      src="ho-oh-4-color.png"
      alt="4-color Game Boy conversion"
      caption="Game Boy Color (4 colors)"
      class="max-w-full lg:max-w-xs"
  >}}
  {{< figure
      src="ho-oh-8-color.png"
      alt="8-color Game Boy Color conversion"
      caption="Game Boy Color (8 colors)"
      class="max-w-full lg:max-w-xs"
  >}}

## ✨ Features

- **🎨 Hardware-Accurate for GB Studio (GBC & GB)**: Creates images that work perfectly with GB Studio's specifications, that run on real hardware. Abides by both Game Boy and Game Boy Color palette restrictions, with options for any number of colors.

- **📱 Easy-to-Use Interface**: Simple sliders and options make conversion intuitive for everyone, with a focus on GB Studio developers.

- **📐 Smart Resizing**: Automatically resizes your images while keeping the proper Game Boy aspect ratio.

- **🧩 Tile Optimization**: Optionally reduces the number of unique tiles to help stay within Game Boy memory limitations.

## 🎯 Perfect For
- **🎮 GB Studio Developers**: Create game assets that perfectly match hardware limitations without learning complex image editing techniques.

- **✏️ Pixel Artists**: Quickly prototype how your art would look on actual Game Boy hardware.

- **🕹️ Retro Enthusiasts**: Transform modern photos into hardware-accurate Game Boy graphics.

## 🎬 How It Works

The tool handles all the technical aspects for you:
- 📐 Resizes your image to Game Boy resolution
- 🖌️ Applies authentic color palette reduction, if desired
- 🧮 Optimizes for tile restrictions, if desired
- 💾 Provides ready-to-use output for GB Studio

{{< figure
    src="ui.png"
    alt="Gameboy Image Converter interface"
    caption="The simple interface makes conversion easy for everyone"
    >}}

## How to do hardware-accurate palette limits?
- Check the "Limit to 4 colors per tile, 8 palettes (For GB Studio development only)" box
- Adjust the sliders to your desired number of colors
- Click "Convert Image"

{{< alert icon="triangle-exclamation" cardColor="#cc7000" iconColor="#1d3557" textColor="#f1faee" >}}
If your image has harsh color edges, you may want to try reducing the number of colors or changing the <em>Quantization Method</em>
{{< /alert >}}

{{< figure
    src="color-limit.png"
    alt="Palette Limits"
    caption="Limiting the image to 10 total colors across 8 different 4-color palettes"
>}}

## How to do hardware-accurate tile limits?
- Check the "Reduce to 192 unique 8x8 tiles (Not needed for LOGO scene mode)" box
- Adjust the "Tile similarity threshold" slider. This will merge tiles that share the % of pixels specified. Raising or lowering this will change how tiles are merged & reduced.
- Click "Convert Image"

{{< alert icon="triangle-exclamation" cardColor="#cc7000" iconColor="#1d3557" textColor="#f1faee" >}}
If you see <em>"Out of spare tiles"</em> in the console, then your image is too complex for the similarity threshold you've set, and you should lower it.
{{< /alert >}}
<br />
{{< alert icon="triangle-exclamation" cardColor="#cc7000" iconColor="#1d3557" textColor="#f1faee" >}}
If you see <em>"Consider increasing Tile Similarity Threshold"</em> in the console, then your image is too simple for the similarity threshold you've set, and you should consider raising it.
{{< /alert >}}

{{< figure
    src="tile-limit.png"
    alt="Tile Limits"
    caption="Merging all tiles that share 90% of their pixels"
>}}

{{< alert "wand-magic-sparkles" >}}
Try it yourself at [gameboy.prodigle.dev](https://gameboy.prodigle.dev)
{{< /alert >}}
<br />
{{< github repo="sirprodigle/gameboy-image-converter" >}}

