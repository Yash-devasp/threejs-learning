# Creating a Basic Scene

Let's start by creating a basic `index.html` page

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

Now add a `script.js` file in the html file and also add the `three.min.js` from the three.js build folder.

```html
<script src="./script.js"></script>
<script src="./three.min.js"></script>
```

## 4 Elements to get started

- A scene that will contain objects
- Some objects
- A Camera
- A Renderer

### Scene

- It is like a container
- We put objects, models, layers, etc. in it
- At some point we will ask Three.js to render the scene

```javascript
// creating a Scene
const scene = new THREE.Scene();
```

### Objects

Objects can be many things

- Primitive Geometries
- Imported Models
- Particles
- Lights
- etc.

We will start with a simple red cube.

#### We need to create a [Mesh](https://threejs.org/docs/index.html?q=mesh#api/en/objects/Mesh)

It is a combination of a geometry (the shape) and a material (how it looks)

We are starting with a [BoxGeometry](https://threejs.org/docs/index.html?q=box#api/en/geometries/BoxGeometry) and a [MeshBasicMaterial](https://threejs.org/docs/index.html?q=mesh#api/en/materials/MeshBasicMaterial)

#### Geometry

Let us Instantiate a [BoxGeometry](https://threejs.org/docs/index.html?q=box#api/en/geometries/BoxGeometry)

```javascript
// Creating a Cube
const geometry = new THREE.BoxGeometry(1, 1, 1);
```

The first three parameters correspond to the box's size

#### Material

Now, moving further we have to instantiate a [MeshBasicMaterial](https://threejs.org/docs/index.html?q=mesh#api/en/materials/MeshBasicMaterial)

```javascript
const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });
```

There are multiple ways to express colors
- 0xff0000
- #ff0000
- 'red'
- [Color](https://threejs.org/docs/index.html?q=color#api/en/math/Color)

Now using the two objects of `Geometry` and `Material`, we can instantiate the mesh and add it to the scene
```javascript
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

### Camera

- It is not visible
- It serves as a point of view when doing a render
- We can have multiple cameras and can switch between them
- The cameras are of different types
- We are going to use [PerspectiveCamera](https://threejs.org/docs/index.html?q=pers#api/en/cameras/PerspectiveCamera)

```javascript
// creating a camera
const camera = new THREE.PerspectiveCamera(...);
scene.add(camera);
```

### The Field of View
- Vertical Vision Angle
- In degrees
- Also called `fov`

For this example we will use a 75 degrees angle

```javascript
const camera = new THREE.PerspectiveCamera(75, ...);
scene.add(camera);
```

#### Aspect Ratio

The width of the render divided by the heightof the render. We don't have a render yet but we can decide on a now.

Create a `sizes` object containing temporary values.

```javascript
// sizes
const size = {
  width: 800,
  height: 600
};
```

```javascript
const camera = new THREE.PerspectiveCamera(75, size.width / size.height);
scene.add(camera);
```
### Renderer

- Render the scene from the camera's point of view
- Result drawn into a canvas
- A canvas is a HTML element in which you can draw stuff
- Three.js will use webgl to draw the render inside this canvas
- We can create it or we can let Three.js do it

create the `<canvas>` element before the script element in the `index.js` with a `webgl` class.

```html
<canvas class="webgl"></canvas>
```

Create the [WebGLRenderer](https://threejs.org/docs/index.html?q=we#api/en/renderers/WebGLRenderer) with an empty object

```javascript
const renderer = new THREE.WebGLRenderer({
  ...
});
```

Use `document.querySelector(...)` to retrieve the canvas we created in the html and pass the `canvas` variable.

```javascript
const canvas = document.quesrySelector('canvas.webgl');
const renderer = new THREE.WebGLRenderer({
  canvas: canvas
});
```

Use the `setSize(...)` method to update the size of the render

```javascript
renderer.setSize(size.width, size.height);
```

## First Render

Call the `render(...)` method on the `renderer` with `scene` and the `canvas` as parameters

```javascript
renderer.render(scene, camera);
```

Now nothing will be visible because the canvas is inside the cube.

We need to mode the camera backward.

To transform an object, we can use the following properties
- `position`
- `rotation`
- `scale`

The position property is also an object with `x`, `y` and `z` properties.

Three.js considers the forward/ backward axes to be `z`.

Move the camera backward before doing the render

```javascript
camera.position.z = 3;
```
