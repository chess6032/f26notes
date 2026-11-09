2026 11 05  

# Review

## Modeling Primitives (Lec. 16)

### Calculating polygon normals

### 

- Consistently specify the vertices of your polygons in a certain order.

## 3D Rendering (Lec. 17)

- Obj space.
- World space.
- Object-to-world transform.

## Transformation hierarchies (Lec. 18)

- *Why?* Lots of geometry is moved in relation to other geometry.
  - (Still has to be $\boldsymbol T \boldsymbol R \boldsymbol S$ or $\boldsymbol T \boldsymbol S \boldsymbol R$ mtx mult.)
- Building the hierarchy.
- Transformation stacks.
  - Push/pop mtx.
    - Keep track of a mtx at each level of the hierarchy.
    - Push going down.
    - Pop going up. 

## 3D Rendering (Lec. 19)

- Camera coordinate space.
  - Origin, axes.
- Creating camera space.
  - Specify LookFrom, LookAt, and ViewUp.
  - Creating coord axes:
    - $\mathbf e_3$ (Z): LookAt - LookFrom.
    - $\mathbf e_1$ (X): e3 x ViewUp.
    - $\mathbf e_2$ (Y): e1 x e3.
    - ^ Make sure to NORMALIZE these camera vectors.
    - (NOTE: double check the order.)
- World-to-camera transform.
  - Translate to world origin.
  - Use rotation mtx from cam coord axes.
- ^ (Graham-Schmidt orthogonalization.)

## Clip Mtx (Lec. 19)

- View frustrum
  - FOV.
  - Relation btwn focal length, zoom, FOV, and aspect ratio.
    - One FOV (btwn x or y) will be given, with the other's (y or x) aspect ratio. 
    - $\text{zoom} = 1 / \tan(\text{FOV}/2)$
  - Near & far clipping planes.
  - View frustrum culling.
- Clip mtx.
  - MEMORIZE THIS.
  - Clip mtx handles perspective, too. Because it goes straight to canonical view.
- Clip tests.
  - Compare each coord with $w$: If *any* of the point's $x$, $y$, or $z$ components are NOT in the interval $(-w, w)$ (or $[-w, w]$&mdash;just be consistent).
- Canonical view.
  - If *not* doing $z$-buffering or anything like that, you can drop the $z$ component. 
- Homogeneous divide.

## Canonical to Actual screen space (Lec. 19)

- Scale $x$ and $y$ to screen width/height, respectively.
  - (Width & height are both $2$ in the canonical view, so we have to divide by $2$.)
- Flip $y$ axis (depending on the 2D drawing coord system).
- Move the origin from teh center of the screen to the upper left corner.
  - (Depending on the 2D drawing coord system... may be in bottom left, or center.)
- MEMORIZE THE MTX THAT DOES THIS.


## Visibility (Lec. 21)

- Back-face culling.
  - Don't render polygons facing in the same/similar direction as camera.
    - HOW TO DETERMINE: Compare camera's direction, $\mathbf {\hat e}_3$, to polygon's normal, $\mathbf {\hat n}$: $\hat{\mathbf {e}}_3 \cdot \hat{\mathbf n} < 0 ?$
  - Assumes one-sided polygons.
- Occlusion testing
  - Painter's algorithm: Draw everything back-to-front.
  - Z-buffering: In addition to image buffer, have a z-buffer. When drawing a point, compare the $z$ value stored in the same position in the z-buffer. If the new point is in front of the previous, draw it; otherwise, don't.
    - (If they're matching, pick either the one there or the one not there&mdash;just be consistent.)
    - Produces same order regardless of the order in which polygons are drawn&mdash;*unless* two polygons are on top of each other.
  - Ray-casting: Shoot a ray out from the viewpoint. First thing it hits is th
- Accceleration using bounding boxes: Enclose objects in some shape (typically boxes). When rendering objects, first check the *box* (which is&mdash;or should be&mdash;cheaper) to calculate a hit.

## Lighting & Shading (Lec. 22)

> [!TIP]
> Definitely finish HW 8 before the exam!!!!!!!!!!!

- Ambient, direct, indirect lighting.
- Irradiance: Amount of **light per area**.
- Basic lighting geometry-f-asihdf pajfpoaiwf
- asdfpo asjdipsjdf
- Squared distance law: Intensity of light varies based on square of distance from light source.
  - We use a lumination equation that *doesn't* take this into consideration.
- Simple phong model.
- Ambient light.
  - USES: 
    - object's color, $O_d$
    - ambient light color, $I_a$
    - ambient coefficient, $k_a$.
- Diffuse reflection.
  - USES: ???
  - Lambert's law.
    - $\mathbf {\hat n}$: surface normal.
    - $\mathbf {\hat l}$: direction from point to light source.
  - Independent of view direction.
- Specular reflection.
  - USES: 
    - $k_d$: Specular coefficient.
    - $I_p$: Color of light source.
    - $O_s$: Specular color of object.
    - $k_\text{gloss}$: Glossiness of object.
    - $R$: Light vector reflected across normal...or something.
  - Dependent on view directiono.
- Bidirectional reflectance distribution function (BRDF).
  - *Uhhhh oops I forgor*.
- Shadow computation.

## Shading models (Lec. 23)

Three ways:

- Flat
  - Illum. equation: Calculated **once per *polygon***.
  - Same normal for entire polygon.
- Gouraud
  - Illum. equation: Calculated **once per *vertex***.
  - Interpolating *colors* btwn vertices. 
- Phong
  - Illum. equation: **once per *pixel***.
  - Interpolating *normals* btwn vertices.
- Texture mapping.
- Bump mapping.

## Advanced Lighting Models (Lec. 24)

- Recursive ray tracing.
  - Basic idea: Bounce rays over and over again, then sum each bounce's color.
  - Environment maps.
  - Strengths: Really good lighting, specular reflections, transparencies. 
  - Weaknesses: 
    - Slow af&mdash;takes a long time to compute.
    - Doesn't get *diffuse* reflections.
- Radiosity.
  - Basic idea: 
  - Strengths: Diffuse reflections.
  - Weaknesses: Doesn't get specular reflections.
- Combining radiosity + recursive reflection.
  - Possible, but slow af af.
- Forward rendering appraoches: Proton mapping, bidirectional path tracing.

## General

- Full rendering pipeline.
  - Expect 2-3 problems on various stages of the pipeline.
- 2D/3D transformation mtxs.
  - Rotation, translation, scale.
- Mtx mult.
- Cross prod.
- Doct prod.
- Right-handed vs. left-handed coord systems.
- CALCULATORS ALLOWED! You may use your phone&mdash;but only if you *pinky swear* you're using it *only* for a calculator.
- You do NOT get to take your own notes in, but you will be given a page of notes from Parris.