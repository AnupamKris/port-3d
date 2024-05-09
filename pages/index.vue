<template>
  <div class="index" ref="home">
    <NavBar />

    <div class="container home">
      <div class="name">
        <h1>
          <div class="char bit" v-for="char in 'I\'M'">
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
      <div class="info">
        <p class="fl">Generative AI</p>
        <p class="fl">& Web Development</p>
        <p class="fl">Enthusiast</p>

        <div class="cir fl"></div>
        <div class="bor fl">
          <div class="lines"></div>
          <div class="lines"></div>
          <div class="lines"></div>
          <div class="lines"></div>
          <div class="lines"></div>
          <div class="lines"></div>
          <div class="lines"></div>
        </div>
        <div class="box fl"></div>
        <div class="box2 fl"></div>
      </div>
      <div class="bg-skill">
        <div class="row r1">
          <div class="skill v">Vue</div>
          <div class="skill">.</div>
          <div class="skill r">React</div>
          <div class="skill">.</div>
          <div class="skill j">JavaScript</div>
          <div class="skill">.</div>
          <div class="skill s">SCSS</div>
          <div class="skill">.</div>
          <div class="skill j">Python</div>
          <div class="skill">.</div>
          <div class="skill g">Flask</div>
          <div class="skill">.</div>
        </div>
        <div class="row r2">
          <div class="skill g">GenAI</div>
          <div class="skill">.</div>
          <div class="skill g">LLMs</div>
          <div class="skill">.</div>
          <div class="skill g">ML</div>
          <div class="skill">.</div>
          <div class="skill g">DL</div>
          <div class="skill">.</div>
          <div class="skill g">WebScraping</div>
          <div class="skill">.</div>
        </div>
        <div class="row r3">
          <div class="skill v">SQL</div>
          <div class="skill">.</div>
          <div class="skill f">Firebase</div>
          <div class="skill">.</div>
          <div class="skill g">DigitalOcean</div>
          <div class="skill">.</div>
          <div class="skill f">Blender</div>
          <div class="skill">.</div>
          <div class="skill g">SocketIO</div>
          <div class="skill">.</div>
          <div class="skill v">OpenCV</div>
          <div class="skill">.</div>
        </div>
      </div>
    </div>
    <div class="container about">
      <h1>About Me</h1>
      <p>
        Lorem ipsum, dolor sit amet consectetur adipisicing elit. Magnam commodi
        beatae officiis nisi at sunt aliquid est, quam eum ratione molestias
        eligendi quaerat cumque consequatur harum? Aspernatur explicabo
        laboriosam, porro suscipit aperiam nam sed nisi est culpa eum dicta.
        Cumque, veniam soluta aspernatur id ratione dolorem. Odio iusto earum
        velit omnis vel, debitis minima eaque quod, ducimus laborum itaque magni
        animi eum aperiam cupiditate reprehenderit. Ab fuga explicabo, aut dolor
        optio officiis ut neque sed debitis exercitationem nisi praesentium
        ratione, error sint atque. Nulla magnam at eum esse maxime commodi
        reiciendis eveniet saepe. Omnis cupiditate recusandae dolor soluta
        placeat. Dolorum.
      </p>
      <div class="grid" @click="flipstate"></div>
    </div>
  </div>
</template>

<script setup>
import { gsap } from "gsap";
import { horizontalLoop } from "@/assets/helpers";
import { Flip } from "gsap/Flip";
import { TextPlugin } from "gsap/all";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";

gsap.registerPlugin(Flip, ScrollTrigger, ScrollToPlugin, TextPlugin);

const flipstate = () => {
  let el = document.querySelector(".about");

  gsap.to(el, {
    duration: 2,
    text: "Hi, I'm Anupam Krishna",
  });
};

onMounted(() => {
  let loop1 = horizontalLoop(".r1 .skill", {
    repeat: -1,
    speed: 2,
  });
  let loop2 = horizontalLoop(".r2 .skill", {
    repeat: -1,
    reversed: true,
  });
  let loop3 = horizontalLoop(".r3 .skill", {
    speed: 3,
    repeat: -1,
  });
  gsap.to(".char", {
    duration: 0.3,
    y: 0,
    stagger: 0.05,
    delay: 3,
    ease: "back.out(1.7)",
  });
  gsap.to(".bg-skill", {
    duration: 0.5,
    opacity: 0.09,
    delay: 3.5,
  });
  gsap.to(".info", {
    duration: 0.5,
    right: -20,
    delay: 3.5,
  });
  gsap.to(".fl", {
    duration: 0.5,
    y: 0,
    delay: 3.5,
  });

  gsap.to(".bg-skill", {
    yPercent: 50,
    scrollTrigger: {
      scrub: true,
    },
  });
});
</script>

<style lang="scss" scoped>
.index {
  background: #141414;
  // background: url("@/assets/filler.png") repeat;

  height: 100vh;
  width: 100%;

  overflow-y: scroll;
  // scroll-snap-type: y mandatory;
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
  display: flex;
  flex-direction: column;

  height: 100%;
  width: 100%;

  scroll-snap-align: start;
}

.home {
  pointer-events: none;

  height: 100vh;

  position: relative;
  top: 0;
  left: 0;

  overflow: hidden;
  .name {
    height: auto;
    margin-top: auto;
    margin-left: 15px;
    z-index: 1;
  }

  .fl {
    transform: translateY(100%);
  }

  .info {
    font-size: 1rem;
    color: white;
    // text-align: center;
    margin-top: 20px;
    font-family: "BitxMap", sans-serif;
    border: 1px solid white;

    width: 200px;

    padding: 20px;
    position: absolute;

    bottom: 110px;
    right: -170px;

    transform: rotateZ(-90deg);
    z-index: 1;
    // overflow-y: hidden;

    .cir {
      width: 45px;
      height: 45px;
      border-radius: 50%;
      background: #fff;
      position: absolute;
      top: 0px;
      right: -60px;
    }
    .box {
      width: 10px;
      height: calc(100% + 2px);

      background: #fff;
      position: absolute;
      top: -1px;
      left: -20px;
    }
    .box2 {
      width: 3px;
      height: calc(100% + 2px);

      background: #fff;
      position: absolute;
      top: -1px;
      left: -30px;
    }

    .bor {
      width: 45px;
      height: 45px;
      // border-radius: 50%;
      border: 2px solid #fff;
      position: absolute;
      bottom: 0px;
      right: -60px;

      display: flex;

      flex-direction: column;
      align-items: center;

      overflow: hidden;

      .lines {
        width: 150%;
        height: 2px;
        background: #fff;

        transform: rotateZ(45deg);
        margin-top: 5px;
      }
    }
  }

  .bg-skill {
    position: absolute;
    bottom: 0;
    left: 0;

    font-size: 256px;
    color: #9c9c9c;

    z-index: 0;
    opacity: 0;
    height: calc(100% - 100px);
    width: 100%;

    display: flex;
    flex-direction: column;
    justify-content: flex-end;

    font-family: "BitxMap", sans-serif;

    .row {
      display: flex;
      height: 220px;
      line-height: 200px;
    }

    .r1 {
      transform: translateX(-160px);
    }
    .r3 {
      transform: translateX(-260px);
    }

    .skill {
      width: fit-content;
      // line-height: 200px;
      height: 200px;
      // font-style: italic;
    }

    .v {
      color: #41b883;
    }

    .r {
      color: #61dafb;
    }

    .j {
      color: #f7df1e;
    }

    .s {
      color: #c65353;
    }
    .g {
      color: #646464;
    }
    .f {
      color: #ffc038;
    }
  }

  h1 {
    font-size: 12rem;
    color: white;

    text-align: center;
    display: flex;

    height: 12rem;

    overflow: hidden;

    display: flex;
    align-items: center;

    .char {
      font-family: "Syncopate", sans-serif;
      font-weight: 700;
      min-width: 15px;
      height: 100%;

      display: flex;
      justify-content: center;
      align-items: center;

      transform: translateY(12rem);
      //text-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);
    }

    // .bit {
    //   font-family: "BitxMap", sans-serif;
    // }
  }

  * {
    pointer-events: all;
  }
}

.about {
  // background: #141414;
  color: white;
  display: flex;
  flex-direction: column;
  height: 100vh;

  padding-top: 80px;

  font-family: "BitxMap", sans-serif;

  h1 {
    font-size: 64px;
  }

  .grid {
    height: 300px;
    display: flex;
    .gridi {
      padding: 10px;
    }
  }

  .col {
    flex-direction: column;
  }
}
</style>
