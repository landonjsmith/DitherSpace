# DitherSpace 
**DitherSpace** is an open-source vanilla JS port of the original cyberspace.online dither algorithm.  
It provides real-time dithering with two algorithm options: GPU-accelerated Bayer matrix dithering and CPU-based Floyd-Steinberg error diffusion. Features include grayscale quantization, adjustable pixelation, and customizable color mapping.

Original Vue-Bayer algorithm courtesy of @unremarkablegarden on GitHub.  
Translated to vanilla JavaScript by Landon J. Smith

---

## Features

### Image Loading
- Upload any local image (`.png`, `.jpg`, etc.)
- Includes a built-in sample generator for testing

### Algorithm Selection
Choose between two dithering methods:
- **Bayer 8×8 (WebGL-based)**: Fast, GPU-accelerated ordered dithering with real-time performance
- **Floyd-Steinberg (CPU-based)**: Higher-quality error diffusion dithering with more organic results

### Real-Time Controls
All rendering updates instantly (Bayer on GPU, Floyd-Steinberg on CPU).

| Control | Description |
|--------|-------------|
| **Algorithm** | Switches between Bayer and Floyd-Steinberg dithering |
| **Pixel Size** | Adjusts blocky pixelation amount (1–20 px) |
| **Dither Amount** | Blends between pure quantization and full dithering |
| **Bit Depth** | Quantizes grayscale levels (1–8 bits) |
| **Contrast** | Applies contrast adjustment before dithering |
| **Scale** | Scales canvas output resolution |
| **Foreground Color** | Color of "ink" pixels |
| **Background Color** | Color of empty/light pixels |

---

## Dithering Algorithms

### Bayer 8×8 (WebGL)
DitherSpace's Bayer algorithm uses a hybrid of:
- **8×8 Bayer ordered dithering**
- **Pseudo-random noise** (for texture variation)
- **Grayscale reduction** via bit-depth quantization
- **Per-pixel thresholding** based on user-controlled dither strength

The Bayer process:
1. Convert source pixel to grayscale  
2. Apply contrast adjustment  
3. Fetch Bayer threshold for current pixel  
4. Optionally mix in small random noise  
5. Quantize based on selected bit depth  
6. Blend quantized and dithered results depending on **Dither Amount**  
7. Map final value to **FG / BG** output colors  

Rendering is handled entirely in a WebGL fragment shader for real-time performance.

### Floyd-Steinberg (CPU)
The Floyd-Steinberg algorithm provides higher-quality error diffusion:
- **Error propagation**: Distributes quantization error to neighboring pixels
- **Organic dithering**: Creates more natural-looking patterns than ordered dithering
- **CPU processing**: Slower than Bayer but produces superior quality for final outputs

The Floyd-Steinberg process:
1. Convert source pixel to grayscale
2. Apply contrast adjustment
3. Quantize pixel based on bit depth
4. Calculate quantization error
5. Distribute error to adjacent unprocessed pixels:
   - 7/16 to the right pixel
   - 3/16 to the bottom-left pixel
   - 5/16 to the bottom pixel
   - 1/16 to the bottom-right pixel
6. Map final value to **FG / BG** output colors

---

## Image Output
- Final result is drawn to an HTML `<canvas>`  
- Users can export the dithered output as a **PNG file**
- All preferences are saved to localStorage and persist between sessions
