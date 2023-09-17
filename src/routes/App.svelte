<script>
import {
  CylinderGeometry,
  Color,
  Mesh,
  MeshBasicMaterial,
  PerspectiveCamera,
  TextureLoader,  // A class to load a texture
  Scene,
  WebGLRenderer,
  HemisphereLight,
  DirectionalLight,
} from 'three';
import { onMount } from 'svelte';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

// just waiting for your beautiful creations!
let angle = 0; // A variable to store the rotation angle
// Create a texture map
let textureLoader = new TextureLoader();
async function init() {
// Get a reference to the container element that will hold our scene
const container = document.querySelector('#scene-container');
// create a Scene
const scene = new Scene();
// Set the background color
scene.background = new Color('skyblue');
// Create a camera
const fov = 35; // AKA Field of View
const aspect = container.clientWidth / container.clientHeight;
const near = 0.1; // the near clipping plane
const far = 100; // the far clipping plane
const camera = new PerspectiveCamera(fov, aspect, near, far);
// every object is initially created at ( 0, 0, 0 )
// move the camera back so we can view the scene
camera.position.set(0, 0, 0);
// create a geometry
//const geometry = new CylinderGeometry( 1, 1, 2, 25 ); 
// create a default (white) Basic material
//let marbleTexture = textureLoader.load('/textures/textura.jpg');
//const material = new MeshBasicMaterial({ map: marbleTexture });
// create a Mesh containing the geometry and material
//const cube = new Mesh(geometry, material);
//Adding a fix position avoid rotation
//cube.position.set(0, -10, -10);
//cube.scale.set(2, 2, 2);
//add the mesh to the scene
//I need to fix the cube in position horizontal
//cube.position.set(0, 0, 0);
//cube.rotation.x = -260;
//scene.add(cube);
//Load the model GLTF
const loader = new GLTFLoader();
loader.load('/models/realistic_human_heart.glb', function(gltf) {
      gltf.scene.scale.set(1, 1, 1);
      scene.add(gltf.scene);
})  
//I need a light for my GLTF model, but not Hemisfere
const light = new HemisphereLight(0xffffff, 0x444444);
light.position.set(0, 20, 0);
scene.add(light);
light.intensity = 1;
light.castShadow = true;
//I need a ligth to see the GLTF original colors
const light2 = new HemisphereLight(0xffffff, 0x444444);
light2.position.set(0, -20, 0);
scene.add(light2);
light2.intensity = 1;
light2.castShadow = true;
//I need a ligth to see the GLTF all meshes
const light3 = new HemisphereLight(0xffffff, 0x444444);
light3.position.set(0, 0, 20);
scene.add(light3);
light3.intensity = 1;
light3.castShadow = true;
//I need a directional light
const light4 = new DirectionalLight(0xffffff, 1);
light.position.set(0, 10, 10);
light.color.setHSL(0.6, 1, 0.95); // set color temperature to 5500K
scene.add(light4);
//I need more directional ligth
const light5 = new DirectionalLight(0xffffff, 1);
light.position.set(-10, 20, -10);
light.color.setHSL(0.6, 1, 0.95); // set color temperature to 5500K
scene.add(light5);
// create the renderer
const renderer = new WebGLRenderer({ antialias: true })
// next, set the renderer to the same size as our container element
renderer.setSize(container.clientWidth, container.clientHeight);

// start the loop
renderer.setAnimationLoop(() => {
renderer.render(scene, camera);
});
// finally, set the pixel ratio so that our scene will look good on HiDPI displays
renderer.setPixelRatio(window.devicePixelRatio);
// add the automatically created <canvas> element to the page
container.append(renderer.domElement);
// render, or 'create a still image', of the scene
const controls = new OrbitControls( camera, renderer.domElement );
//controls.update() must be called after any manual changes to the camera's transform
camera.position.set( 0, 10, 10 );
controls.update();
function animate() {
	requestAnimationFrame( animate );
	// required if controls.enableDamping or controls.autoRotate are set to true
	controls.update();
	renderer.render( scene, camera );
  angle -= 0.01; // Increase the angle by 0.01 radians on each frame
  //cube.rotation.y = angle; // Rotate the cylinder
  //cube.position.x = angle
 }
  animate();
}

onMount(() => {
  init();
  function onResize() {
    console.log('You resized the browser window!');
  }
  window.addEventListener('resize', onResize);
  onResize();
});
</script>

<style>
    body {
  /* remove margins and scroll bars */
  margin: 0;
  overflow: hidden;

  /* style text */
  text-align: center;
  font-size: 12px;
  font-family: Sans-Serif;

  /* color text */
  color: #444;
}

h1 {
  /* position the heading */
  position: absolute;
  width: 100%;

  /* make sure that the heading is drawn on top */
  z-index: 1;
}

#scene-container {
  /* tell our scene container to take up the full page */
  position: absolute;
  width: 100%;
  height: 100%;

  /*
    Set the container's background color to the same as the scene's
    background to prevent flashing on load
  */
  background-color: skyblue;
}

</style>
<div id="scene-container">
    <!-- Our <canvas> will be inserted here -->
</div>