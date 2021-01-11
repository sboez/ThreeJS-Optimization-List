# ThreeJS Performances Optimization List

:purple_heart: <span style="color:#c936ff"> __Feel free to add your knowledge. I was based on my experience and the links cited at the bottom__</span>

To improve performance of your app, you generally want to reduce the amount of draw calls and the complexity of your geometries and materials

## General Tips :first_quarter_moon_with_face:

- Object creation in JavaScript is expensive, so don’t create objects in a loop.
- Always use [BufferGeometry](https://threejs.org/docs/#api/en/core/BufferGeometry) instead of Geometry as it’s faster.

&nbsp;
## Simple Tips :full_moon_with_face:

- ### Camera :movie_camera:

Make your __camera__ frustum as small as possible for better performance. You can set a smaller camera on load based on screen resolution the following code sets the camera for mobile, <= 1080px & > 1080px

``` 
let FOV, FAR;
let NEAR = 400;

// Mobile camera
if (window.innerWidth <= 768) {
  FOV = 50;
  FAR = 1200;
// 769px - 1080px screen width camera
} else if (window.innerWidth >= 769 && window.innerWidth <= 1080) {
  FOV = 50;
  FAR = 1475;
// > 1080px screen width res camera
} else {
  FOV = 40;
  FAR = 1800;
}

this.camera = new THREE.PerspectiveCamera(
  FOV,
  window.innerWidth / window.innerHeight,
  NEAR,
  FAR
)
```

- ### Lights :bulb:
Use as few __lights__ as possible in the scene. Avoid adding and removing lights from your scene, since this requires the WebGLRenderer to recompile the shader programs. [HemisphereLight](https://threejs.org/docs/#api/en/lights/HemisphereLight) & [DirectionalLights](https://threejs.org/docs/#api/en/lights/DirectionalLight) are lightweight.


- ### Renderer :milky_way:

__Antialiasing__
   

Disabling __antialiasing__ resulted in a considerable performance bump on macs with retina displays. As retina / high end displays have such a high pixel density there is very little visual difference between having AA on/off. Below is a hacky solution for checking for a high end display and toggling antialiasing.

```
let pixelRatio = window.devicePixelRatio;
let AA = true;

if (pixelRatio > 1) {
  AA = false;
}

this.renderer = new THREE.WebGLRenderer({
  antialias: AA,
  powerPreference: "high-performance",
})
```

__PowerPreference__

Selecting __high-performance__ when creating renderer. This may make a users system choose the high-performance GPU, in multi-GPU systems.

`powerPreference: 'high-performance'`

&nbsp;
## Advanced Tips :new_moon_with_face:

### Materials :8ball:

High quality materials will be slower.

To slowest / highest quality :
- [MeshStandardMaterial](https://threejs.org/docs/#api/en/materials/MeshStandardMaterial)
- [MeshPhongMaterial](https://threejs.org/docs/#api/en/materials/MeshPhongMaterial)
- [MeshLambertMaterial](https://threejs.org/docs/#api/en/materials/MeshLambertMaterial)
- [MeshBasicMaterial](https://threejs.org/docs/#api/en/materials/MeshBasicMaterial)

### 3D Models :monkey_face:

If you want to import a 3D model in your scene, it's recommand to have a .glb or .gltf format, it's made for the web.
Your model need to have regular mesh to be the most light possible.

&nbsp;
## Need help :question:

### Measuring performance

You can monitor high level performance improvements by implementing the [stats module](https://github.com/mrdoob/stats.js/) to show:

- FPS Frames rendered in the last second. The higher the number the better.
- MS Milliseconds needed to render a frame. The lower the number the better.
- MB MBytes of allocated memory.

&nbsp;
## Sources :books:

- [The Big List of three.js Tips and Tricks](https://discoverthreejs.com/tips-and-tricks/)
- [Tips & Tricks for speeding up Three.js](https://attackingpixels.com/tips-tricks-optimizing-three-js-performance/)