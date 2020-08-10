<template>
    <canvas id="canvas" width="600" height="400"></canvas>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import * as THREE from 'three';

@Component
export default class RotateBox extends Vue {
    private name = 'RotateBox';

    private scene = new THREE.Scene();

    private renderer: (THREE.WebGLRenderer|null) = null;

    private camera = new THREE.PerspectiveCamera(75, 600 / 400, 0.1, 1000);

    private light = new THREE.DirectionalLight(0xffffff);

    private geometry = new THREE.BoxGeometry(1, 1, 1);

    private material = new THREE.MeshNormalMaterial();

    private cube = new THREE.Mesh(this.geometry, this.material);

    public mounted() {
      console.log('mounted');
      const $canvas = document.getElementById('canvas');
      // canvasを後付けで設定する方法あったら教えてほしいー
      this.renderer = new THREE.WebGLRenderer({
        antialias: true,
        canvas: $canvas as HTMLCanvasElement,
      });

      this.camera.position.set(0, 0, 2);
      this.light.position.set(0, 0, 10);
      this.scene.add(this.cube);
      this.scene.add(this.light);

      this.animate();
    }

    public animate() {
      requestAnimationFrame(this.animate);

      this.cube.rotation.x += 0.02;
      this.cube.rotation.y += 0.02;

      this.renderer!.render(this.scene, this.camera);
    }
}
</script>
