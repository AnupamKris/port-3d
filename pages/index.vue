<template>
  <div class="home" ref="home">
    <div class="container">
      <!-- <button @click="spinBox = !spinBox">ROT</button> -->
      <div class="name">
        <h1>
          <div class="char" v-for="char in 'I\'M'">
            {{ char }}
          </div>
        </h1>

        <h1>
          <div class="char" v-for="char in 'ANUPAM'">
            {{ char }}
          </div>
        </h1>
        <h1>
          <div class="char" v-for="char in 'KRISHNA'">
            {{ char }}
          </div>
        </h1>
      </div>
    </div>
    <canvas
      ref="canvas3d"
      id="canvas3d"
      @click="handleCanvasClick"
      @mousemove="handleMouseOver"
      @wheel="handleMouseWheel"
    ></canvas>
  </div>
</template>

<script setup>
import { gsap } from "gsap";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
import python from "@/assets/python.glb";

const loader = new GLTFLoader();

const home = ref(null);
const canvas3d = ref(null);
const currentHoveredObject = ref(null);

const spinBox = ref(false);

let scene, camera, renderer, raycaster, mouse;

const geometries = ref([]);
const scrollAccumulator = ref(0);

const rotateBox = () => {
  geometries.value.forEach((geometry) => {
    geometry.rotation.x += 0.01;
    geometry.rotation.y += 0.01;
  });
};

onMounted(() => {
  scene = new THREE.Scene();
  camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  renderer = new THREE.WebGLRenderer({ canvas: canvas3d.value, alpha: true });
  renderer.setSize(window.innerWidth, window.innerHeight);

  raycaster = new THREE.Raycaster();
  mouse = new THREE.Vector2();

  const geometry = new THREE.BoxGeometry(8, 0.1, 5);
  // const geo2 = new THREE.BoxGeometry(1, 1, 1);
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const material2 = new THREE.MeshBasicMaterial({ color: 0x0088ff });

  const cube = new THREE.Mesh(geometry, material);
  const cube2 = new THREE.Mesh(geometry, material2);

  // const text = new Text();
  // scene.add(text);

  // text.text = "Anupam Krishna";
  // text.fontSize = 1;
  // text.color = 0xffffff;
  // text.font = "/GreatVibes-Regular.ttf";
  // text.position.x = -5;
  // text.position.z = -3;

  // text.rotation.x = -1.57;

  // const textGeo = new THREE.TextGeometry("Hello", {
  //   font: new THREE.FontLoader().load(
  //     "https://cdn.jsdelivr.net/gh/mrdoob/three.js/examples/fonts/helvetiker_regular.typeface.json"
  //   ),
  //   size: 1,
  //   height: 0.1,
  // });

  cube.addEventListener("click", () => {
    console.log("clicked");
    // spinBox.value = !spinBox.value;
  });

  geometries.value.push(cube);
  geometries.value.push(cube2);
  const pythonLogo = loader.load(
    python,
    (gltf) => {
      scene.add(gltf.scene);
      geometries.value.push(gltf.scene);
      console.log(gltf.scene);
      console.log(geometries.value[2].scale);
      geometries.value[2].scale.set(10, 10, 10);
    },
    (xhr) => {
      console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
    }
  );

  cube["hoverable"] = true;
  cube2["hoverable"] = false;

  // scene.add(cube);
  scene.add(cube2);

  cube.rotation.x = 0;

  cube2.position.x = 10;
  cube2.position.y = 15;
  cube2.position.z = 10;

  camera.position.z = 7;
  camera.position.y = 0;
  camera.lookAt(0, 0, 0);

  gsap.to(".char", {
    duration: 0.3,
    y: 0,
    stagger: 0.05,
    delay: 0.5,
  });

  function animate() {
    if (spinBox.value) {
      rotateBox();
    }
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }
  animate();

  setTimeout(() => {
    gsap.to(camera.position, {
      x: 0,
      y: 5,
      z: 0,
      duration: 1,
      onUpdate: () => {
        camera.lookAt(0, 0, 0);
      },
    });
  }, 1500);
});

const handleCanvasClick = (event) => {
  console.log("hadnling canvas  click");
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;

  raycaster.setFromCamera(mouse, camera);

  const intersects = raycaster.intersectObjects(scene.children);

  if (intersects.length > 0) {
    intersects[0].object.dispatchEvent({ type: "click" });
  }
};

const handleMouseOver = (event) => {
  // change color of the object

  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
  let xtilt = (mouse.x - 0.5) * 0.02;
  let ytilt = (mouse.y - 0.5) * 0.1;

  // scene.children.forEach((child) => {
  //   // mouse parrallax rotate objects based on position
  //   // gsap.to(child.rotation, {
  //   //   x: ytilt,
  //   //   y: xtilt,
  //   //   duration: 1,
  //   // });
  // });

  raycaster.setFromCamera(mouse, camera);

  const intersects = raycaster.intersectObjects(scene.children);

  if (intersects.length > 0) {
    // change color  + 1
    if (
      currentHoveredObject.value?.uuid != intersects[0].object.uuid &&
      intersects[0].object.hoverable
    ) {
      if (currentHoveredObject.value) {
        currentHoveredObject.value.material.color.set(0x0088ff);
        gsap.to(currentHoveredObject.value.scale, {
          x: 1,
          y: 1,
          z: 1,
          duration: 1,
        });
      }

      currentHoveredObject.value = intersects[0].object;
      currentHoveredObject.value.material.color.set(0x00ff00);
      gsap.to(currentHoveredObject.value.scale, {
        x: 1.2,
        y: 1.2,
        z: 1.2,
        duration: 1,
      });
    }
  } else {
    if (currentHoveredObject.value) {
      currentHoveredObject.value.material.color.set(0x0088ff);
      gsap.to(currentHoveredObject.value.scale, {
        x: 1,
        y: 1,
        z: 1,
        duration: 1,
      });
      currentHoveredObject.value = null;
    }
  }
};

const handleMouseWheel = (event) => {
  scrollAccumulator.value += event.deltaY;

  if (scrollAccumulator.value >= 300 && camera.position.y != 18) {
    gsap.to(camera.position, {
      x: 10,
      y: 18,
      z: 10,
      duration: 2,
      // onComplete: () => {
      //   gsap.to(camera.position, {
      //     y: 18,

      //     duration: 2,
      //   });
      // },
    });
  } else if (scrollAccumulator.value < 300 && camera.position.y != 5) {
    gsap.to(camera.position, {
      x: 10,
      y: 25,
      z: 10,
      duration: 2,
      onComplete: () => {
        gsap.to(camera.position, {
          x: 0,
          y: 5,
          z: 0,
          duration: 2,
        });
      },
    });
  }
};
</script>

<style lang="scss" scoped>
.home {
  background: rgb(0, 71, 85);
  background: radial-gradient(
    circle,
    rgba(0, 71, 85, 1) 0%,
    rgba(2, 0, 36, 1) 100%
  );

  height: 100vh;
  width: 100%;
}

#canvas3d {
  height: 100%;
  width: 100%;

  position: absolute;
  left: 0;
  top: 0;
  z-index: 0;
}

.container {
  pointer-events: none;
  display: flex;
  flex-direction: column;

  height: 100%;
  width: 100%;

  position: absolute;
  top: 0;
  left: 0;

  z-index: 1;

  .name {
    height: auto;
    margin-top: auto;
    margin-left: 15px;
  }

  h1 {
    font-size: 10rem;
    color: white;

    text-align: center;
    display: flex;

    height: 10rem;

    overflow: hidden;

    display: flex;
    align-items: center;

    .char {
      font-family: "Raleway", sans-serif;
      font-weight: 400;
      min-width: 15px;
      height: 100%;

      display: flex;
      justify-content: center;
      align-items: center;

      transform: translateY(10rem);
      //text-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);
    }
  }

  * {
    pointer-events: all;
  }
}
</style>
