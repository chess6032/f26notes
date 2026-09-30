

# Matrices

## Determinant

### Geometric interpretation

The determinant gives you the area spanned by the matrix's two column vectors.

### Properties

TODO

## Linear Independence

### Definition

A set of vectors is linearly ***dependent*** if *at least one* of them can be expressed as a linear combination of the others.

**The vectors for a basis of a coordinate system MUST be linearly independent**. 

### Singular Matrices

$$|M^{-1}| = \frac 1 {|M|}$$

If $|M| = 0$, then $M$ is ***singular***.

Singular matrices have ***linearly dependent ROWS***.

### Rank

The rank of a matrix is the **number of lin. indep. rows.**.

- When used as transformations, mtx w/ *full rank* transform to the full space.
- Singular mtx have *insufficient* ranks.

## Orthogonal Mtx

Two (square) mtx are said to be orthogonal iff:

$$MM^T = I \Rightarrow M \text{ is orthogonal}$$

* Implies rows are orthonormal vectors

Orthogonal matrices are easily invertible:

$$M^{-1} = M^T$$

And this implies

$$|M| = |M^{-1}| = 1$$

### Orthogonal &hArr; Rotation

- All rotation mtx are orthogonal.
- All orthogonal mtx are rotations.

# 3D

If you take a picture of a scene with a vcam, you have two problems to solve:

1. What point in 3D is visible at each 2D point (pixel) in the projected image?
2. ^ What color is the light coming from that point as it reaches the color?

## Applications, jobs, & industries

### 3D Models

#### Approaches for creating 3D models

- By hand (w/ 3D software)
  - Very flexible. You can make whatever model you want bc you have complete control. But it can be quite time-consuming and tedious.
- Scanning (something that exists in the real world)
  - A lot easier. But you're limited by the resolution of the scan.
  - Not very flexible. If there's anything wrong with the scan, you have to go in and tweak the vertices by hand.
- Image-based (from photographs)
  - Algorithmic

In general, 3D modelling is quite time-consuming, which means it's expensive.

### 3D Animation

#### Steps

1. Modeling
1. Rigging
1. Animation
1. Physical Simulation
1. Lighting (or "shading")
    - Turns out, this makes a big difference to what people take away from a scene.
    - Some people's entire jobs is just lighting.
1. Rendering
1. Compositing

Very time-consuming process.

### Special Effects

Usually a combination of *real filmed elements* and *computer-generated* elements.

There are some things you must do to make it look right.

- Vcam must be at *exactly the same position* as the real one was.
- Position of CGI elements *must exactly match* the positions of the real world.
- Lighting *must match exactly* the lighting of the real world.

Special effects often involves *removing* things from your scenes in addition to adding them.

### Other subfields

#### Nonphotorealistic Rendering

- Stylized rendering, etc.
- e.g., technical illustration in a 3D modelling software for *engineers*.
- Used a lot for data visualization.

#### Data visualization

There's a lot of data out there that is hard to see what the data is telling you.

## Upcoming

- Basics of virtual cameras.
- Perspective projection,
- Modeling primitives (points, lines, polygons, etc.).
- 3D rendering geometry. (Lab 7&mdash;you'll implement it yourself.)
- Intro to OpenGL. (Lab 5)
- Hierarchical transformations. (Lab 6)
- Visibility. (Lab 8)
- Lighting. (Lab 8)
