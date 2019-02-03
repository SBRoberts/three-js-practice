<template>
  <div id="app" ref="app">
  </div>
</template>

<script>
import Vue from 'vue';
import {TweenMax} from 'gsap/TweenMax';
import * as THREE from 'three';

export default {
  name: 'app',
  components: {
  },
  data() {
    // instantiate scene
    const scene =  new THREE.Scene();

    // initialize camera, rednerer
    const camera =  new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
    const renderer =  new THREE.WebGLRenderer();

    // geometry represents the shape we're building
    const boxGeometry =  new THREE.BoxGeometry( 30, 30, 30 );
    const torusGeometry =  new THREE.TorusGeometry( 70, 20, 50, 100, 6.3 );
    const planeGeometry =  new THREE.PlaneGeometry( window.innerWidth, window.innerHeight * 3, 10, 10 );

    // material refers to the material of the geometry
    const boxMaterial =  new THREE.MeshPhongMaterial( {
      color: 0xff2caf,
      shininess : 40,
      precision: 'highp',
      wireframe: false,
    });
    const torusMaterial =  new THREE.MeshPhongMaterial( {
      color: 0xffd800,
      shininess : 40,
      precision: 'highp',
      wireframe: false,
    });
    const planeMaterial =  new THREE.MeshLambertMaterial( {
      color: 0xb1b1b1,
      precision: 'highp',
      wireframe: false,
    });

    // edges is taking the current geometry to draw lines
    // const edges = new THREE.EdgesGeometry( boxGeometry );
    const edges = new THREE.EdgesGeometry( boxGeometry );
    const line = new THREE.LineSegments( edges, new THREE.LineBasicMaterial( { color: 0x000000, linewidth: 30 } ) );
    const cube =  new THREE.Mesh( boxGeometry, boxMaterial );
    const torus =  new THREE.Mesh( torusGeometry, torusMaterial );
    torus.position.z = -100;
    cube.position.z = -100;
    line.position.z = -100;
    const plane =  new THREE.Mesh( planeGeometry, planeMaterial );
    const light1 = new THREE.AmbientLight(0xffffff,0.5);
    const light2 = new THREE.PointLight(0xffffff,0.5);
    return {
      scene,
      camera,
      renderer,
      boxGeometry,
      boxMaterial,
      cube, 
      torus,
      line,
      light1,
      light2,
      plane
    }
  },
  methods : {
    animate() {  
      requestAnimationFrame(this.animate);  
      // this.torus.rotation.x += 0.005; 
      this.torus.rotation.y += 0.005;

      this.cube.rotation.x += 0.005; 
      this.cube.rotation.y += 0.0075;

      this.line.rotation.x += 0.005; 
      this.line.rotation.y += 0.0075;
      this.renderer.render( this.scene, this.camera ); 
    } 
  },
  mounted() {
    this.$refs.app.appendChild( this.renderer.domElement );
    this.renderer.setSize( window.innerWidth, window.innerHeight );
    this.scene.add( this.cube );
    this.scene.add( this.line );
    this.scene.add( this.torus );
    this.scene.add( this.plane );
    this.scene.add( this.light1 );
    this.scene.add( this.light2 );
    this.plane.rotation.x = 80;
    this.plane.position.y = -80;

    this.camera.position.z = 150;
    this.animate()
  },
}
</script>

<style lang="scss">
@import 'src/styles/index.scss';
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  align-self: flex-start;
  // max-height: 100vh;
  height: 100%;
  width: 100vh;
  canvas {
    width: 100%;
    height: 100%;
  }
}
</style>
