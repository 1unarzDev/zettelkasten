---
aliases:
  - Three
  - Web 3d
tags:
  - Seed
references:
  - "[three.js journey](https://threejs-journey.com/)"
---
[[Programming]], [[Websites]], [[Art]], [[Math]]
2026-07-20 22:31

A **mesh** is a combination of a **geometry** (shape) and **material** (appearance).
```javascript
import * as THREE from 'three';

const scene = new THREE.Scene()

const geometry = new THREE.BoxGeometry(1, 1, 1)
const material = new THREE.MeshBasicMaterial({ color: "#ff0000" })
const mesh = new THREE.Mesh(geometry, material)
scene.add(mesh)
```

You can have multiple cameras in a scene, and you can toggle between them.

Don't forget to add things to the scene with 
```javascript
const camera = new THREE.PerspectiveCamera() // fov degrees, aspect (width/height of viewport)

scene.add(mesh)
scene.add(camera)
```

The renderer renders the scene from the camera's point of view. It results in a drawn canvas.

Objects in Three have position, rotation, and scale properties that can be set. The z-axis is considered forward.

```javascript
const canvas = document.querySelector('.draw') // this is how you get an element from the dom
const renderer = new THREE.WebGLRenderer({
    canvas: canvas
})
renderer.setSize(sizes.width, sizes.height)
renderer.render(scene, camera)
```
