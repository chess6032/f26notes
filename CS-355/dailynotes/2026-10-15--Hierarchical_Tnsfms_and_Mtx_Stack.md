2026 10 26  
CS 355  
Paris  
Hierarchical Transformations & Matrix Stack  

> [!NOTE]
> These are the notes I took while looking at the slides for Lecture 18, from Oct. 15, which I missed. The day today however is Oct. 26.

# Hierarchical Transformations

So far, we've been talking about transformations as strictly object-to-world. But in a hierarchical transformation, **each object's transformation mtx transforms from its object coords to its *parents* obj coords.** (i.e., from the object's coord system to its parent's coord system.) So now it's all about **object-to-*parent*** transformations.

Consider the case of a *chair* that's inside a *classroom* that's inside a *building* that's on a *campus* that's in *the world*. The obj-to-world transformation for a vertex of that chair, $\mathbf p$, would be thus:

```math
\mathbf p_{\text{world}} = \boldsymbol T_{\text{campus}} \boldsymbol T_{\text{building}} \boldsymbol T_{\text{classroom}} \boldsymbol T_{\text{chair}} \mathbf p
```

Each $\boldsymbol T_{\text{obj}}$ is $\text{obj}$'s transformation matrix&mdash;translation, scale, & rotation&mdash;from its coord system to its parent's. e.g., $\boldsymbol T_{\text{classroom}}$ transforms from the obj coords of the classroom to the obj coords of the building&mdash;the classroom's parent. $\boldsymbol T_{\text{campus}}$ is what finally transforms to world coords.

To satisfy my desire for generality, I'll say this: For an object with $N$ ancestors&mdash;that is, its parent is transformation $N$&mdash;its obj-to-world transformation would look like this: 

```math
\mathbf p_{\text{world}} = \boldsymbol T_{1} \boldsymbol T_{2} \dots \boldsymbol T_{N} \boldsymbol T_{\text{obj}} \mathbf p_{\text{obj}}
```

## Leveraging composition

Now let's move away from pure theory and talk about how to optimize the math&mdash;that is, how to reduce the number of computations.

Suppose in that classroom you have *multiple* chairs. Would you want to recalculate that full list of matrices for each chair? No! While each chair may have its own chair-to-room mtx, they all share the same room-to-building-to-campus-to-world matrix! 

Fortunately, because mtx mult is associative, we can multiply everything above $\boldsymbol T_{\text{chair}_n}$ first, save it, then apply it to each chair. Or, more generally, **we can use the same parent-to-world mtx between siblings** in the hierarchy.

```math
\mathbf p_{\text{world}} = \bigg(\Big((\boldsymbol T_{\text{campus}} \boldsymbol T_{\text{building}}) \boldsymbol T_{\text{classroom}}\Big) \boldsymbol T_{\text{chair}_n}\bigg) \mathbf p
```

## Transformation stacks

TODO

<!-- 
### Example

Suppose you have a bunch of cars you want to draw. Each car is in a different spot, so it needs its own car-coords-to-parent-coords transformation matrix. On the other hand, these cars are all the same model, so each car has the same four tire-coords-to-car-coords transformation matrices.

```
// UNDER THE HOOD

C: Mtx = I        // current matrix
stack: Stack[Mtx] // transformation stack

void push(void):
  // "remember this spot."
  stack.push(C)

void pop(void):
  // "go back to where I was."
  stack.pop()
  C = stack.top()

void concatenate(M: Mtx):
  // "go deeper---move into child (M) coordinate system."
  C = C @ M

// USAGE

cars: List[Mtx] = [ /* car to parent */]
tires: (Mtx, Mtx, Mtx, Mtx) = */ tire to car */

void drawCars(void):
  push() // push car's parent (whatever that may be...)
  foreach i in range(cars.length):
    drawCar(i)
  pop()

void drawCar(i):
  push()
  concatenate(cars[i])
  drawCarBody()
  foreach j in range(tires.length):
    drawTire(j)
  pop()

void drawTire(j):
  push()
  concatenate(tireTransform[j])
  drawOneTire()
  pop()
```

### General

```
abstract class Object:
  private transform: Mtx; // obj-to-parent transformation
  private children: List[Object] = [];

  protected abstract drawMe(); // overridden in subclasses

  public void draw():
    push()
    concat(this.transform)
    
    drawMe()
    foreach child in this.children:
      child.draw()
    
    pop()
``` -->
