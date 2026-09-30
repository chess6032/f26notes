# Matrices and 3D Graphics

## 2D Transformation Matrices

* Remember that **scaling**, **rotation**, **shearing**, and **reflection** are all ***relative to the origin*** (or in the latter's case, an axis).

### 2D Rotation

```math
\mathbf R(\theta) = \begin{bmatrix}
    \cos \theta & -\sin \theta \\
    \sin \theta & \cos \theta
\end{bmatrix}
```

### 2D Scaling

```math
\begin{bmatrix}
    s_x & 0 \\
    0   & s_y
\end{bmatrix}
\begin{bmatrix}
    x \\
    y
\end{bmatrix}
=
\begin{bmatrix}
    s_x \space x \\
    s_y \space y
\end{bmatrix}
```

### 2D Shearing

```math
\begin{bmatrix}
    1 & s_x \\
    0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
    x \\
    y
\end{bmatrix}
= 
\begin{bmatrix}
    x + s_x \space y \\
    y
\end{bmatrix}
```

```math
\begin{bmatrix}
    1 & 0 \\
    s_y & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y
\end{bmatrix}
=
\begin{bmatrix}
    x \\
    y + s_y \space x
\end{bmatrix}
```

```math
\begin{bmatrix}
    1 & s_x \\
    s_y & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y
\end{bmatrix}
=
\begin{bmatrix}
    x + s_x \space y \\
    y + s_y \space x
\end{bmatrix}
```

### 2D Reflection

```math
\begin{bmatrix}
    -1 & 0 \\
    0  & 1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y
\end{bmatrix}
=
\begin{bmatrix}
    -x \\
    y
\end{bmatrix}
```

```math
\begin{bmatrix}
    1 & 0  \\
    0 & -1
\end{bmatrix}
\begin{bmatrix}
    x \\
    y
\end{bmatrix}
=
\begin{bmatrix}
    x \\
    -y
\end{bmatrix}
```

### 2D Translation with homogenous coords

```math
\begin{bmatrix}
    1 & 0 & t_x \\
    0 & 1 & t_y \\
    0 & 0 & 1
\end{bmatrix}
```

When combining with another transformation matrix $\mathbf M$, *do that transformation first*,and you'd end up with this:

```math
\begin{bmatrix}
    \mathbf M & \mathbf t \\
    \mathbf {0}^T & 1
\end{bmatrix}
```


