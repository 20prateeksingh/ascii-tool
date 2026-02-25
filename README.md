# ASCII Asset Generator

## Overview
The **ASCII Asset Generator** is a standalone, client-side static web application that converts uploaded images into highly customizable ASCII art directly in your browser.

This tool is a lightweight, zero-dependency utility contained entirely within a single `index.html` file, designed to make creating text-based assets incredibly simple.

## Features
- **Instant Image to ASCII Conversion**: Leverages HTML5 Canvas to read pixel data, calculate perceived brightness, and map it to an ASCII character ramp.
- **Adjustable Parameters**:
  - **ASCII Ramps**: Choose from carefully selected presets (Fintech, Max, Binary, Blocks, Squares, Circles, Triangles, Braille) or provide your own custom character map.
  - **Color Picker**: Change the text color of the generated ASCII output.
  - **Resolution**: Adjust the width of the ASCII output while maintaining the correct aspect ratio natively.
  - **Image Processing Adjustments**: Built-in sliders for Brightness, Contrast, and Saturation to prep your image before conversion. This allows you to force the contrast into specific bands needed for your chosen ASCII ramp characters.
- **Export Options**: 
  - **Copy to Clipboard**: Instantly copy the raw ASCII text for use in your source code, terminal interfaces, or text documents.
  - **Download PNG**: Generate and save a high-quality rendered `png` image of the ASCII art exactly as it appears.

## Usage
No backend server, `npm install`, or build step is required!

1. Clone or download this repository.
2. Open `index.html` in any modern web browser.
3. Click **Choose Image...** to upload an image from your device.
4. Select your desired **ASCII Ramp** and tailor your **Resolution** and **Lighting** adjustments to fit the image perfectly. 
5. Click **Copy Text** or **Download PNG** to export your creation.

## Technical Details
- **Architecture**: Vanilla HTML, CSS, and JavaScript. Zero external dependencies.
- **Styling**: Modern, responsive layout using CSS Grid/Flexbox and CSS variables for a clean UI.
- **Image Processing Execution**: Uses `getImageData` from a hidden `<canvas>` to read live pixel values after applying dynamic CSS filters (`brightness`, `contrast`, `saturate`).
- **Font Rendering Proportions**: Employs a fixed `FONT_ASPECT` ratio correction algorithm for monospace typography scaling, ensuring the output shape remains identical to the input geometry.
