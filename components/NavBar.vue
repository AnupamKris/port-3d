<template>
  <nav>
    <div class="loader">{{ parseInt(loadPercent) }}%</div>
    <div class="logo">AK</div>
    <div class="items">
      <div class="item">Home</div>
      <div class="item">About</div>
      <div class="item">Achievements</div>
      <div class="item">Projects</div>
      <div class="item">Contact</div>
    </div>
  </nav>
</template>

<script setup>
import { gsap } from "gsap";
const loadPercent = ref(0);

onMounted(() => {
  let tl = gsap.timeline();
  gsap.to(loadPercent, {
    duration: 1,
    value: 100,
  });
  tl.to(".loader", {
    duration: 1,
    width: "100%",
    ease: "none",
  });
  tl.to("nav", {
    duration: 0.5,
    top: 20,
    left: 20,
    delay: 0.5,
  });
  tl.to(".loader", {
    duration: 0.5,
    opacity: 0,
    delay: 0.5,
  });

  tl.to(".logo", {
    duration: 0.2,
    y: 0,
    opacity: 1,
    stagger: 0.1,
  });
  tl.to(".item", {
    duration: 0.2,
    y: 0,
    opacity: 1,
    stagger: 0.1,
  });
});
</script>

<style lang="scss" scoped>
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;

  color: white;
  border: 1px solid white;
  background: #16161655;

  width: 700px;

  position: fixed;
  top: calc(50% - 25px);
  left: calc(50% - 350px);

  height: 50px;
  z-index: 5;

  overflow: hidden;

  .logo {
    font-family: BitxMap;
    font-size: 2rem;
    text-transform: uppercase;

    cursor: pointer;
    height: 100%;

    margin: 0 10px;

    display: flex;
    justify-content: center;
    align-items: center;

    opacity: 0;
    transform: translateY(50px);
  }

  .items {
    display: flex;
    justify-content: space-between;
    align-items: center;

    // width: 300px;
    overflow: hidden;

    .item {
      font-family: BitxMap;
      text-transform: uppercase;

      cursor: pointer;
      margin: 15px 20px;
      opacity: 0;
      transform: translateY(50px);
    }
  }

  .loader {
    position: absolute;
    top: 0;
    left: 0;

    height: 100%;
    width: 0;

    display: flex;
    justify-content: center;
    align-items: center;
    background: #888888;

    font-family: BitxMap;
  }
}
</style>
