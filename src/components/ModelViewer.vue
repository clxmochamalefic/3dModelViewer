<template>
    <canvas id="canvas" width="800" height="600"></canvas>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

@Component
export default class ModelViewer extends Vue {
    private name = 'ModelViewer';

    private scene = new THREE.Scene();

    private renderer: (THREE.WebGLRenderer|null) = null;

    private camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000,
    );

    private light = new THREE.DirectionalLight(0xffffff);

    private geometry = new THREE.BoxGeometry(1, 1, 1);

    private material = new THREE.MeshNormalMaterial();

    private cube = new THREE.Mesh(this.geometry, this.material);

    public mounted() {
      console.log('mounted');
      const $canvas = document.getElementById('canvas');
      if ($canvas != null) {
        $canvas.setAttribute('width', window.innerWidth.toString());
        $canvas.setAttribute('height', window.innerHeight.toString());
      }
      // canvasを後付けで設定する方法あったら教えてほしいー
      this.renderer = new THREE.WebGLRenderer({
        antialias: true,
        canvas: $canvas as HTMLCanvasElement,
      });

      this.camera.position.set(0, 0, 10);
      this.light.position.set(0, 0, 10);

      const gltfLoader = new GLTFLoader();
      // gltfLoader.load('./../assets/models/house_map.bin', (gltf) => {
      // gltfLoader.load('/src/assets/models/house_map.gltf', (gltf) => {
      gltfLoader.load('/threeAssets/house_map.gltf', (gltf) => {
        this.scene.add(gltf.scene);
      }, (xhr) => {
        const loaded = (xhr.loaded / xhr.total) * 100;
        console.log(`${loaded}% loaded`);
      }, (error) => {
        console.error('An error happened');
        console.error(error);
      });

      // this.scene.add(this.cube);
      this.scene.add(this.light);

      this.animate();
    }

    public animate() {
      requestAnimationFrame(this.animate);

      if (this.renderer != null) {
        this.renderer.render(this.scene, this.camera);
      }
    }
}
</script>
