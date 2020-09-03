<template>
  <div class="scene" ref="scene" />
</template>

<script lang="ts">
import {
  PerspectiveCamera,
  Scene,
  WebGLRenderer,
  Mesh,
  ExtrudeBufferGeometry,
  Shape,
  SpotLight,
  MeshStandardMaterial
} from 'three';
import { Component, Vue } from 'vue-property-decorator';

@Component<MyScene>({
  mounted() {
    const sceneElement = this.$refs.scene as Element;
    this.renderer.setSize(window.innerWidth, window.innerHeight);
    this.renderer.setClearColor(0xffffff, 0);
    this.renderer.shadowMap.enabled = true;
    sceneElement.appendChild(this.renderer.domElement);

    this.camera = new PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    this.camera.position.set(0, 0, 80);

    const x = 0;
    const y = 0;
    const heartShape = new Shape();

    heartShape.moveTo(x + 5, y + 5);
    heartShape.bezierCurveTo(x + 5, y + 5, x + 4, y, x, y);
    heartShape.bezierCurveTo(x - 6, y, x - 6, y + 7, x - 6, y + 7);
    heartShape.bezierCurveTo(x - 6, y + 11, x - 3, y + 15.4, x + 5, y + 19);
    heartShape.bezierCurveTo(x + 12, y + 15.4, x + 16, y + 11, x + 16, y + 7);
    heartShape.bezierCurveTo(x + 16, y + 7, x + 16, y, x + 10, y);
    heartShape.bezierCurveTo(x + 7, y, x + 5, y + 5, x + 5, y + 5);

    const extrudeSettings = {
      amount: 8,
      bevelEnabled: true,
      bevelSegments: 5,
      steps: 5,
      bevelSize: 2,
      bevelThickness: 1
    };
    const geometry = new ExtrudeBufferGeometry(heartShape, extrudeSettings);
    const mesh = new Mesh(
      geometry,
      new MeshStandardMaterial({
        color: 0xff0000,
        refractionRatio: 0.01,
        metalness: 1,
        roughness: 1
      })
    );
    mesh.receiveShadow = true;
    mesh.castShadow = true;
    mesh.rotation.x = 3;

    const light = new SpotLight(0xffa95c, 4);
    light.position.set(-50, 50, 50);

    this.scene.add(mesh);
    this.scene.add(light);

    const animate = () => {
      requestAnimationFrame(animate);

      mesh.rotation.y += 0.01;

      this.renderer.render(this.scene, this.camera);
    };

    animate();
  }
})
export default class MyScene extends Vue {
  private camera!: PerspectiveCamera;
  private scene = new Scene();
  private renderer = new WebGLRenderer({ alpha: true });
}
</script>

<style scoped>
.scene {
  width: 100%;
  height: 100%;
}
</style>
