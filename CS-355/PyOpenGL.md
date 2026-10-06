# PyOpenGL

I'm using PyOpenGL [version 3.1.10](https://pyopengl.sourceforge.net/documentation/manual-3.0/).

## `glOrtho()`

Multiplies the current matrix w/ an orthographic matrix.

```c
void glOrtho(
    GLdouble left, 
    GLdouble right, 
    GLdouble bottom, 
    GLdouble top, 
    GLdouble zNear, 
    GLdouble zFar
);
```

- PARAMS:
  - `left`, `right`: Coords for left & right **vertical clipping planes**.
  - `bottom`, `top`: Coords for btm & top **horizontal clipping planes**.
  - `zNear`, `zFar`: Distances to the nearer & farther depth clipping planes. 
    - Make these negative if you want the plane to be behind the viewer.

## `gluPerspective()`

```c
void gluPerspective(
    GLdouble fovy,
    GLdouble aspect,
    GLdouble zNear,
    GLdouble zFar
);
```

Sets up a perspective projection matrix.

- PARAMS:
  - `fovy`: FOV angle, in degrees, in the $y$ direction.
  - `aspect`: Aspect ratio that determines the FOV in the $x$ direction.
    - Ratio of $x$ (width) to $y$ (height).
  - `zNear`: Distance from viewer to near clipping plane (always positive).
  - `zFar`: Distance from the viewer to the far clipping plane.

## `glRotated()`

```c
void glRotated(
    GLdouble angle,
    GLdouble x,
    GLdouble y,
    GLdouble z
);
```

Multiply current mtx by a rotation mtx.

- PARAMS:
  - `angle`: Angle of rotation, in degrees.
  - `x`, `y`, `z`: $x$/$y$/$z$ coords of a vector forming the *axis of rotation*.
    - e.g., to rotate around the $x$ axis, you would input `x=1, y=0, z=0`.

> [!NOTE]
> There's also `glRotatef()`. The parameters and operation are the same, the only difference is that `glRotatef()` uses `float`s while `glRotated()` uses `double`s.

## `glTranslated()`

```c
void glTranslated(
    GLdouble x,
    GLdouble y,
    GLdouble z
);
```

Multiply current matrix by translation matrix.

- PARAMS:
  - `x`, `y`, `z`: Translation.

That is, $\mathbf t = [x, y, z]^T$, and this function multiplies the current matrix by $\begin{bmatrix}
\mathbf I_3 & \mathbf t \\
\mathbf 0_3 & 1 
\end{bmatrix}$

> [!NOTE]
> There is also `glTranslatef()`, which is the same function but uses `float`s instead of `double`s.

## `glLoadIdentity()`

```c
void glLoadIdentity();
```

Replaces current mtx w/ the identity mtx.

## `glMatrixMode()`

```c
void glMatrixMode(GLenum mode);
```

Specifies which mtx is the current matrix

- PARAMS:
  - `mode`: Which mtx "stack" to set as the target for subsequent mtx ops. Accepted values are `GL_MODELVIEW`, `GL_PROJECTION`, and `GL_TEXTURE `.
    - (Initial value is `GL_MODELVIEW`.)
    - (If the `ARB_imaging` extension is supported, then `GL_COLOR` is also accepted.)

## `GL_MODELVIEW`

*(This info comes from the [doc](https://www.songho.ca/opengl/gl_transform.html#overview) linked in the Lab 5 spec.)*

**Transformation matrix for object space to camera space.** 

OpenGL has no matrix dedicated to the camera, so you have to simulate the camera yourself. (In other words, OpenGL's camera is always located at $(0,0,0)$, facing down its $-z$ axis.) Camera coordinates are yielded by **multiplying `GL_MODELVIEW` with object coordinates**.

> [!NOTE]
> The docs linked in the assignment spec refer to "camera space" as "eye space". Sometimes it's also called "view space". Isn't that beautiful?

## `GL_PROJECTION`

*(This info comes from the [doc](https://www.songho.ca/opengl/gl_transform.html#overview) linked in the Lab 5 spec.)*

**Transformation matrix for camera space to image plane.** That is, it converts camera coordinates to image coordinates (called "*clipped coordinates*").
