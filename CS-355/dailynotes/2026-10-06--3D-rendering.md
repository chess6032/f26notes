2026 10 06  
CS 355  
Paris  

# Rendering 3D Primitives

POINTS, LINES, & POLYGONS! (Vertices, edges, and faces.)

## Storing model data

Common method:

- List of vertices.
- List of faces bound by vertices (by index).
- Plus some other information about vtx/faces.

Storing faces as *indeces* of the list of vertices helps reduce duplicate data.

## Normals

* A *normal* is important for lighting, determining the face's visibility, etc.
* Sometimes normals are stored alongside the vtx/face data, and other times they're calculated at runtime.
* BE CONSISTENT: Typically use outward-facing normals.

### Calculating face normals from vertices

Some file formats don't store *any* normals.

- Cross product of two of the triangle's edges.
  - You can use any edges so long as you use a consistent *winding order*. (The conventional rule is to save triangle vertices in a counter-clockwise (?) order.)

```math
\hat {\mathbf n} = \frac {(\mathbf v_2 - \mathbf v_1) \times (\mathbf v_3 - \mathbf v_2)}{\lVert(\mathbf v_2 - \mathbf v_1) \times (\mathbf v_3 - \mathbf v_2)\rVert}
```

(If you use this equation and the normals are wrong, you may have to flip your normals. Generally, all the normals in a file are consistent, so you should only have to *either* flip *all* of them or flip none of them.)

### Vertex normals

Some file formats store *vertex* normals instead of face normals. Others store only face normals.

* A vertex normal is calculated by *averaging all the face normals* of the faces that vtx is connected to.
* A face normal is calculated by *averaging all the vertex normals* of the face's vertices.

# Rendering Objects

## Coordinate Spaces

- **Object space**: Where you model your objects.
  - Relative to the object.
  - Choice of origin and coordniates axes is arbitrary.
  - (It's easier to model objects in their own space where they're relative to their own origin and units.)
- **World space**: The scene in which you put all your objects.
  - Relative to the origin of the scene.
  - Objects are *transformed* from object space to world space. This is called ***object-to-world transformation***.
    - **ORDER: Scale &rarr; Rotate &rarr; Translate.**

## Rendering geometry pipeline

1. Transform **object &rarr; world** coords.
2. Transform **world &rarr; camera** coords.
3. **Preprocess** to more efficiently handle things outside of the FOV. 
    - If you're not going to see it, why keep it in your pipeline and spend time doing math on it?
    - (e.g., culling)
    - *(We'll ignore this for now...)*
4. Transform **camera &rarr; pixel** coords. (**Perspective projection** onto image plane.)
5. **Render** pixel coords to screen.

### World-to-camera

Suppose the camera's position is $\mathbf c = (c_x, c_y, c_z)$ and the its orientation is given by a set of basic vectors in world coords: $\{\mathbf e_1, \mathbf e_2, \mathbf e_3\}$. Then transforming world to camera takes two steps:

1. **Translate** everything to be relative to camera position.
2. **Rotate** everything into the camera's viewing rotation. That is, rotate everything so that everything is *down the $z$ axis.*
    - (We need everything down the $z$ axis because that's what our perspective projection mtx is based on.)

*So conceptually we're moving the camera, but in reality we're moving everything so that the camera is at the origin looking down the $z$ axis. (Although in real reality, those operations are equivalent.)*

The matrices for those look like this:

```math
T = \begin{bmatrix}
\mathbf I_3 & \mathbf c \\
\mathbf 0^T & 1 
\end{bmatrix}
```

```math
R = \begin{bmatrix}
\mathbf e_1^T & 0 \\
\mathbf e_2^T & 0 \\
\mathbf e_3^T & 0 \\
\mathbf 0^T & 1
\end{bmatrix}
```

*We'll talk later about how we make these matrices.*

### Putting it together with projection...

(TODO)

