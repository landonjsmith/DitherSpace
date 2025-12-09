# DitherSpace 

**DitherSpace** is an open-source vanilla JS port of the original cyberspace.online dither algorithm.  
It provides real-time, GPU-accelerated dithering using an 8×8 Bayer matrix, grayscale quantization, adjustable pixelation, and customizable color mapping.
Original Vue-Bayer algorithm courtesy of @unremarkablegarden on GitHub.
Translated to vanilla JavaScript by Landon J. Smith

### Features

### Image Loading
- Upload any local image (`.png`, `.jpg`, etc.)
- Includes a built-in sample generator for testing

### Real-Time Controls
All rendering updates instantly on the GPU.

| Control | Description |
|--------|-------------|
| **Pixel Size** | Adjusts blocky pixelation amount (1–20 px) |
| **Dither Amount** | Blends between pure quantization and full Bayer dithering |
| **Bit Depth** | Quantizes grayscale levels (1–8 bits) |
| **Contrast** | Applies contrast adjustment before dithering |
| **Scale** | Scales canvas output resolution |
| **Foreground Color** | Color of “ink” pixels |
| **Background Color** | Color of empty/light pixels |

---

## Dithering Algorithm

DitherSpace uses a hybrid of:
- **8×8 Bayer ordered dithering**
- **Pseudo-random noise** (for texture variation)
- **Grayscale reduction** via bit-depth quantization
- **Per-pixel thresholding** based on user-controlled dither strength

The process:
1. Convert source pixel to grayscale  
2. Apply contrast adjustment  
3. Fetch Bayer threshold for current pixel  
4. Optionally mix in small random noise  
5. Quantize based on selected bit depth  
6. Blend quantized and dithered results depending on **Dither Amount**  
7. Map final value to **FG / BG** output colors  

Rendering is handled entirely in a WebGL fragment shader for real-time performance.

---

Image Output

- Final result is drawn to an HTML `<canvas>`  
- Users can export the dithered output as a **PNG file**

