# Mandelbrot Fractal

`index.html` is a single-file browser tool for exploring the Mandelbrot fractal. It is designed to be simple to run, simple to host, and easy to use on both desktop and mobile devices.

The interface uses a compact control panel together with a square viewing window. The user can enter a center point, a pixel resolution, and a square pixel count, then generate the image. After the image is created, the user can explore it using either **Pan** mode or **Zoom** mode, selected with a single toggle switch beside the **Create Image** button.

## Files

- `index.html` — the main interactive browser tool
- `README.md` — repository overview and usage guide
- `LICENSE` — MIT license text

## What the tool does

The HTML tool lets the user:

- choose the center point using real and imaginary coordinates
- choose the pixel resolution
- choose the square image size in pixels
- create a Mandelbrot image directly in the browser
- watch rendering progress and cancel an in-progress render if desired
- switch between **Pan** mode and **Zoom** mode using a single slider toggle
- tap or click the image to copy the real and imaginary coordinates at that point into the center-point inputs for easy recentering
- drag in **Pan** mode to move around the image
- drag in **Zoom** mode to draw a box and zoom to that region
- zoom by mouse wheel, trackpad gesture, double tap, or pinch gesture on touch devices
- reset all settings to the default starting view

## How to use

### Run locally

1. Download this repository or save the file locally.
2. Open `index.html` in a modern web browser.
3. Adjust the values if desired.
4. Click **Create Image**. While rendering is in progress, a small **Cancel** button appears beside the progress message if you want to stop the render.
5. Use the slider toggle beside **Create Image** to choose either **Pan** or **Zoom** mode.
6. In **Pan** mode, drag the image to move around.
7. In **Zoom** mode, click or touch and drag to draw a zoom box.
8. Tap or click a point in the image to copy its coordinates into the center-point inputs.
9. Click **Create Image** to recenter on that selected point if desired.
10. Use mouse wheel, trackpad scroll, double tap, or pinch to zoom more directly.
11. Use **Reset** at the far right to restore the defaults.
12. Enter new values and click **Create Image** again whenever you want a fresh render.

### Run remotely

Access the tool at:

`https://relationshapez.github.io/mandelbrot/`

## Input meanings

- **Center point (real)**: the real coordinate at the center of the square image. The spin buttons use steps of `0.1`, with a displayed range from `-1.5` to `0.5`, but you may also type more specific values such as `0.251`.
- **Center point (imaginary)**: the imaginary coordinate at the center of the square image. The spin buttons use steps of `0.1`, with a displayed range from `-1` to `1`, but you may also type more specific values if desired.
- **Pixel resolution**: the complex-plane width represented by one screen pixel. The spin buttons change this by `0.001`.
- **Number of pixels**: the width and height of the square image in pixels. The tool no longer enforces an upper limit, but values above about `2000` may become slow on some devices.

A smaller pixel resolution zooms in more deeply. A larger number of pixels gives a sharper image but may take longer to render. For most devices, it is reasonable to stay at `2000` pixels or below.

## What is the Mandelbrot fractal?

The Mandelbrot set is one of the most famous objects in mathematics. It is built from the iteration

`z_{n+1} = z_n^2 + c`

where:

- `z_0 = 0`
- `c` is a complex number
- each point in the image corresponds to a chosen value of `c`

For each point `c` in the complex plane, we repeatedly apply the rule above and watch what happens to the sequence:

- If the values stay bounded, then `c` belongs to the Mandelbrot set.
- If the values grow without bound, then `c` is outside the set.

In the usual picture, points inside the set form the dark central shape, while points outside the set are colored according to how quickly they escape.

The set is famous because it combines a very simple formula with enormously intricate structure. As you zoom in, new spirals, filaments, bulbs, and repeated patterns appear. Many regions resemble miniature copies of the whole set, while other regions contain delicate distortions and unexpected detail.

## Why the picture is interesting

The Mandelbrot set is often used to illustrate several important mathematical ideas:

- **Iteration**: repeating the same rule again and again
- **Complex numbers**: points are described using real and imaginary coordinates
- **Bounded versus unbounded behavior**: some starting choices stay controlled, while others escape
- **Fractal structure**: the boundary contains fine detail at many scales
- **Self-similarity**: parts of the image often resemble the whole, though not always perfectly

This makes the Mandelbrot set a natural visual example of how complicated structure can arise from a very simple rule.

## Interaction modes

The explorer has two main interaction modes.

### Pan mode

In **Pan** mode, dragging moves the current view around the complex plane. This is useful when you want to shift the image without changing the scale.

### Zoom mode

In **Zoom** mode, dragging draws a rectangular box. When you release, the tool zooms so that the selected region becomes the new focus of the picture.

Because the image itself is always square, the tool keeps the same pixel count and computes a new pixel resolution from the selected box. It uses the larger side of the box to determine the new scale, so the selected region fits within the next square rendering.

A simple tap or click, without dragging, still reads off the coordinates of a single point.

## Selecting a point

When you tap or click a point in the image, the tool computes the corresponding complex-plane coordinates for that pixel and copies them into the **Center point (real)** and **Center point (imaginary)** input boxes. This makes it easy to inspect a location and then press **Create Image** to center the next rendering on that point.

## How this explorer renders the image

This tool samples a square grid of points in the complex plane. For each pixel:

1. The code converts the pixel position into a complex number `c`.
2. It starts with `z_0 = 0`.
3. It repeatedly applies `z_{n+1} = z_n^2 + c`.
4. If the magnitude of `z_n` becomes large enough, the point is treated as escaping.
5. The color is then based on how quickly that escape happens.

Points that do not escape within the allowed number of iterations are treated as belonging to the Mandelbrot set for display purposes.

Because the image is built pixel by pixel, larger images and deeper zooms may take longer to render.

## How the number of iterations is chosen

This version of the tool chooses the maximum iteration count automatically from the pixel resolution. The user does not need to set the iteration count manually.

The code uses the rule

```text
zoom = 0.005 / pixelResolution
iterations = 80 + round(18 · log2(max(1, zoom)))
```

and then clamps the result so that it stays between `80` and `1200`.

In practical terms:

- At the default resolution `0.005`, the tool uses `80` iterations.
- Each time the pixel resolution is cut in half, the zoom roughly doubles.
- Each doubling of zoom adds about `18` more iterations.
- The count never drops below `80` and never rises above `1200`.

Examples:

- `pixelResolution = 0.005` → about `80` iterations
- `pixelResolution = 0.0025` → about `98` iterations
- `pixelResolution = 0.00125` → about `116` iterations
- `pixelResolution = 0.000625` → about `134` iterations

This is a rendering heuristic rather than a strict mathematical law. It is meant to balance speed and detail:

- lower zoom levels stay relatively fast
- deeper zoom levels automatically receive more iteration depth
- the interface remains simple because the user does not need to tune another parameter

## Mobile use

The tool is designed to work well on phones and tablets.

- In **Pan** mode, drag with one finger to pan.
- In **Zoom** mode, drag with one finger to draw a zoom box.
- Pinch with two fingers to zoom.
- Double tap to zoom in more quickly.

## Notes

- The image is square.
- Rendering happens entirely in the browser.
- While a render is in progress, a small **Cancel** button appears beside the progress message so that you can stop the computation early.
- Deeper zooms may take longer because the fractal becomes more detailed.
- As you pan, pinch, or box-zoom, the input boxes update to reflect the current view.
- Tapping or clicking the image copies the selected point into the center-point inputs.
- The **Reset** button restores the default center point, pixel resolution, and pixel count.
- **Zoom** is the default interaction mode.
- The interaction-mode toggle is placed beside **Create Image**, while **Reset** is placed at the far right to reduce accidental presses.

## License

Copyright (c) 2026 Alan Miller.

This project is released under the **MIT License**.

See the [`LICENSE`](LICENSE) file for the full license text.
