2026 09 15  
CS 355  
Paris Egbert (he's back, chat)  

# Review of shih from last week

Paris started class by quickly reviewing stuff that Sam went over last week.

- Level point operations: Process each pixel individually.
  - Output is *only* dependent on input pixel.

$$s = T(r)$$

NEED TO KNOWS:

* Brightness, contrast, quantization, and the other basic, simple ones.

# Lab 3 shih

1-byte per channel is fine for *storing* RGB images, but you run into integer overflow if you use that model for image manipulation. 

One way to get arround this is to convert to a higher bit representation after reading the image, then manipulate the image, then convert it back into the lower bit representation. For this lab, you'll do just that:

```py
imread(img, dtype=int.32) # or something like this idk
```

You may only use for loops where necessary (functions 6 & 7).

# Blending, Differencing, and Masking

Now we're going to look image manipulations that use two images to make a single output image.

## Addition & subtraction

Addition looks like a double exposure image.

$$(x, y) = \alpha_1 \text{in}_1(x,y) + \alpha_2 \text{in}_2(x,y)$$

Subtraction is the same but with $-$ instead. Typically though it's more useful to use the absolute difference.

$$(x, y) = \lvert\alpha_1 \text{in}_1(x,y) - \alpha_2 \text{in}_2(x,y)\rvert$$

Applications for subtraction:

- The difference between two images is very useful to see **(visual) differences between images**.
- Real world application: digital subtraction angiography.
- Detecting motion. Take two pictures taken next to in time and use their difference to detect movement.
  - Essential for inserting CGI into a real scene. (The virtual camera must move the same way the physical camera did.)
- Useful for video compression.
  - Only encode the *difference* between frames.
  - e.g., MPEG compresses videos via motion detection/prediction.

## Image averaging

Averaging multiple pictures of the same static scene can help reduce noise. This is similar in principle to taking a low-light image by holding the aperture open.

## Bitwise AND and OR

Useful for masking.

## Alpha blending

Alpha blending uses *per-pixel weights* to blend two images:

$$\text{out}(x,y) = \alpha_1(x,y)\text{in}_1(x,y) + \alpha_2(x,y)\text{in}_2(x,y)$$

(In Lab 3, you will NOT do this, you'll just use a single alpha value for the whole image.)

You want an $\alpha_1$ and $\alpha_2$ that sum to $1$. There was a modified formula derived from the one above that ensured this, but I missed it :(

## Alpha masking

- Blending often uses an *alpha mask*, sometimes called a *matte*.
- The alpha mask is stored with imageg as an extra *alpha channel*.
- APPLICATION: keying (blue screening, green screening, etc.)
  - (Why blue and green? Their hue and saturation makes it easier for them to do.)

# Neighborhood operations

For each pixel, compute an operation based on it and its neighbors (including corners).

These operations are the most common "workhorse" in image manipulation.

## The pixel grid

### Distance

- Euclidean
- "Manhattan": 4-connected.
- "chessboard": 8-connected.

## Spatial filtering ("correlation")

- Multiply each pixel in the neighborhood by a respective weight. Then sum them together.
- Local weights are called a *mask*, or *kernel*.
- i.e., it's an element-wise multiplication of a mask/kernel to a neighborhood, then taking the sum of the result's entries.

$$\text{Image}'(x,y) = \sum_{s=-1}^1 \sum_{t=-1}^1 w(s,t) \space \text{Image}(x + s, y + t)$$

Whilst doing this calculating, you only use the values of the original image.

Most often, the entries in the mask will sum up to one. If they don't, then the output will always be brighter.

### Convolution

- Convolution is almost the exact same as spatial filtering&mdash;except you flip the mask, in both the $x$ and $y$ directions.

$$\text{Image}'(x,y) = \sum_{s=-1}^1 \sum_{t=-1}^1 w(s,t) \space \text{Image}(x - s, y - t)$$

The convolution operator is an **asterisk**: $I' = I * w$

> [!NOTE]
> Most operations produce the same result between convolution vs. correlation (spatial filtering).

### What about the pixels on the edges?

The pixels on the edge of the image course don't have a full neighborhood. Here's a few approaches for fixing that:

- Assume zero. 
  - Not good. Tends to darken borders.
- Assume another constant value, e.g. perhaps avg of entire image.
- Wrap around.
- Assume same as closest pixel still in image.
  - i.e., push out the border pixels one more pixel.
- Don't calculate the border pixels.

## Spatial filtering Applications

- Smoothing
- Blurring
- Sharpening
- Edge detection

### Smoothing

i.e. denoising. But this will blur.

- **Any kernel w/ all positive weights will smooth/blur.**
- To average rather than add, divide each weight in the kernel by the sum of all the kernel's weights.
  - Otherwise, the process will blow up your numbers. 
  - (This would call it *mean filtering*.)
- Kernel can be any size. (Larger kernel means more blurring.)

### Nonlinear smoothing

#### Trimmed means

Perform mean filtering, but remove extreme values before filtering.

#### Median filtering

New value for each pixel is the *median* of the pixel convoluted w/ the kernel.

- More robust to outliers.
  - (Great for "salt and pepper" noise.)
- Tries to respect edges.
  - (Goes w/ local majority.)
- Often rounds corners or loses very small/thin things.

### Bilaterial filtering

Uhhh I missed it. Oops.
