2026 09 17  
CS 355  
Paris Egbert  

# MORE Neighborhood Operations

Remember: Pure convolution just multiplies each neighbor 

Today we'll learn about sharpening & edge detection. Doing so involes *finding or accentuating differences*. This requires a mix of positive *and negative* weights.

## Sharpening

### Unsharp Masking

It *is* a sharpening operation!

- Originated in analog darkrooms.
- KEY IDEA: Mask (subtract) our the blur.
- PROCEDURE:
  - Blur more. ($\bar I$)
  - Subtract from original.
  - Multiply by some fraction. $\alpha$
  - Add back to the original.

$$I' = I + \alpha(I - \bar I)$$


Suppose you let $\alpha = \frac n a$. Then:

$$I' = I + \alpha(I - \bar I) = \frac 1 a (aI + n(I - \bar I))$$

### Tradeoff

Blurring images removes noise. Sharpening an image introduces new noise.

## Edge Detection

Edges are detected by finding (or approximating) ***image derivatives***. Similar to how function derivatives tell you how fast a function is changing at a point, image derivatives tell you how fast pixels are changing at a pixel.

### Derivative approximations

$$\frac d {dt} f(t) = \lim_{h\rarr 0} \frac {f(t+h) - f(t)} h$$

Derivatives can be approximated with finite differences:

$$\frac d {dt} f(t) \approx \frac {f(t+1) - f(t)} 1$$
$$\frac d {dt} f(t) \approx \frac {f(t) - f(t-t)} 1$$
$$\frac d {dt} f(t) \approx \frac {f(t+1) - f(t-1)} 2$$

Those, in order, are called *forward* (right), *backward*, and *central* differences.

### *Partial* derivative approximations

SYMBOL: $\partial$

Images are not lines. They have two directions. How do you differentiate a function with two variables?

$$f(x,y) = 3x^3y^2 + 6xy^2 + 3xy^3+3x$$

With two variables, our function defines a *surface*, not a line. You can take a partial derivative with respect to one of those variables, $x$ or $y$, and it will tell you the rate of change in $x$ or $y$ direction, respectively. When taking one of those partial derivatives, you treat the other variable as a constant.

e.g., taking $\frac {\partial f}{\partial x}$, you would treat $y$ as a constant.

$$\frac {\partial f}{\partial x} = 9x^2y^2 + 6y^2 + 3y^3 + 3$$

$$\frac {\partial f}{\partial y} = 6x^3y + 12xy + 9xy^2$$

With images, we'll take derivative approximations in *both* the $x$ and $y$ directionos.

### Edge Detection Kernels

For edge detection, we'll use the *central* differences&mdash;the difference between one pixel ahead and one pixel behind&mdash;in the $x$ and $y$ direction separately. In doing so, we can see places where pixel values change very suddenly.

#### Prewitt Kernel

You can actually improve that kernel by averaging the vertical entries by the horizontal entires...or something, idrk.

#### Sobel Kernel

Sobel kernels gives the center pixel (i.e., the current pixel) a bit more weight.

After doing the convolution, you'll divide by 4 and then by 2, totalling into dividing by 8.

### Gradients

The gradient ($\nabla$) is a vector of partial derivatives w/ respect to each of a function's inputs.

$$\nabla f = \begin{bmatrix}
\frac {\partial f} {\partial x} \\
\\
\frac {\partial f} {\partial y} \\
\vdots
\end{bmatrix}
$$

- The direction of the gradient is the *direction of greatest increase*.
- The magnitude of the gradient is the *amount* of increase *in that direction*.

Magnitude is found like this:

$$\lVert \nabla f \rVert = \sqrt{
    (\frac {\partial f} {\partial x})^2 +  (\frac {\partial f} {\partial y})^2 + \cdots
}$$

### Putting it all together

1. Sobel kernel in $x$ direction.
2. Sobel kernel in $y$ direction.
3. Magnitude gradient (everywhere? idk).

## Lab 3

- NO for loops for functions 1-4.
- MAYBE for loops for function 5 (Sam & Paris don't remember lul).
- USE for loops for functions 6-8.
