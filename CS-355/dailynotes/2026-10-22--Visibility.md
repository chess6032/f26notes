2026 10 22  
CS 355  
Paris  

# Visibility

There are **two key parts of 3D rendering**:

- What is visible where? (**Visibility**)
  - Where is everything relative to the camera? (World-to-camera transformation)
  - What is within the field of view? (clip mtx/clip tests)
  - What is in front of what? (today)
- What light is coming from there? (**Lighting**)

We'll be talking about that first part&mdash;visibility&mdash;today. In particular, today is about **how to determine what's in front of what**.

## Back-face culling

Back-face culling looks at face normals to determine whether the face is visible to the camera. 

- Rule:
  - If a face's normal points *away* from the camera then that face is *not* visible.
    - (That is, it points in *the same direction* as the camera).
  - Faces whose normals point *towards* the camera *are* visible.
- **Dot product** is used to determine whether a face's normal points in the same direction as the camera.
  - $\hat {\mathbf n} \cdot \mathbf e_3 < 0$ &rArr; render that face.
- Back-face culling assumes your **faces are one-sided**.
- This test can be performed while still in world coords.

## Occlusion testing

Occlusion is about determining which polygons are "in front" of which others. There are three common ways for detecting this:

- Ordered rendering (Painter's alogrithm).
  - If you render things back-to-front, then what's in front will end up in front.
- Image space testing (Z-buffering).
  - For each object, when you draw a pixel, you first check that it is the thing in the front. 
- Ray casting.

### Painter's algorithm

- IDEA: Order objects back-to-front (decreasing $z$), and draw them in that order.
  - This way, front-ward objects are drawn *on top* of fronts.
- CONS: 
  - LOTS of redraws. 
    - Very inefficient.
    - Each pixel is drawn multiple times.
    - You spend time drawing things that may not even be seen.
  - Requires you sort your objects. 
    - Takes time.
    - May not be possible&mdash;that is, some polygons may not strictly be in front of another.
      - e.g., polygons that overlap.
      - e.g., polygons that cut into each other.
      - You *could* find where the polygons overlap/cut and then split them into smaller triangles...but that adds even more complexity.

The painter's alorithm isn't used much anymore, but the idea itself shows up in *lots* of places.

### Z-buffering

- IDEA: When going to draw a pixel, you compare the object you're drawing with the one previously drawn on that pixel: if the one you're currently drawing is closer than the one previously drawn for that pixel, then you draw. Otherwise, you skip that pixel.
- Objects may be drawn in any order.
- To implement, you need to add a **Z-buffer**. 
  - This buffer is the same size as the image.
  - When you draw each pixel, you will draw the pixel to the screen *and add the depth of that object to the pixel's pos in the z buffer*.
    - For a point, $[x/w, y/w, z/w, 1]^T$, $(x/w, y/w)$ gives the coords on the screen/z-buf, and $z/w$ gives the depth. 
- ISSUE: What if two objects have *exactly the same* depth?
  - Well that's a problem...but that rarely happens. "It's not a big deal," Paris says, you just pick on or the other.
  - This is what causes that artifact where things flicker back and forth between two faces that are exactly on top of one another.
- CONS: 
  - Additional memory.
  - Round-off error may be an issue. (You must use floating point.)
  - Nonlinear by depth. (Coarser farther away.)
    - This was on the slides but idk what it means.
  - Makes antialiasing harder.

### Ray Casting

- IDEA: Shoot a ray out from the camera's focal point through the pixel's location on the projection plane.
- Developed by Arthur Appel, 1968.
- CON: 
  - Time consuming.

## Bounding boxes

- *All* of the aformentioned occlusion test techniques can be optimized/accelerated by grouping primitives within **bounding boxes** (or other shapes/volumes).
  - If a bounding box is not within the camera's view frustrum, *all* the objects inside that box are skipped.
  - ^ This way, rather than testing all those faces, you just check a single box first.
- Paris says bounding volumes are used everywhere.
- You may even consider using bounding boxes *hierarchically*. That is, you have bounding boxes within bounding boxes within bounding boxes.  
- TYPES:
  - Axis-aligned. The edges of the box are aligned with the $x$, $y$, $z$ axes. These are the simplest to implement. To create a box, one corner is just the minimum $(x, y, z)$ of all its objects coords, and the opposite corner is the max $(x, y, z)$ coords.
  - Non-axis-aligned. This can get boxes to fit an object more closely. 
  - Other volumes ("bouding volumes") have been tried, but Paris says boxes are just so simple they keep 
