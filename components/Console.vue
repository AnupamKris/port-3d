<template>
  <div class="console" ref="consoleRef">
    <pre v-if="!active">~/ Press ` to toggle console</pre>

    <div class="else" v-else>
      <pre ref="consoleText">{{ headerText }}</pre>
      <div class="entry">
        <pre>{{ currentPath }}</pre>
        <input type="text" />
      </div>
    </div>
  </div>
</template>

<script setup>
const consoleRef = ref(null);
import { Terminal } from "@xterm/xterm";
let term;

const headerText = `
~/ init

  /$$$$$$                                                             
 /$$__  $$                                                            
| $$  \ $$ /$$$$$$$  /$$   /$$  /$$$$$$   /$$$$$$  /$$$$$$/$$$$       
| $$$$$$$$| $$__  $$| $$  | $$ /$$__  $$ |____  $$| $$_  $$_  $$      
| $$__  $$| $$  \ $$| $$  | $$| $$  \ $$  /$$$$$$$| $$ \ $$ \ $$      
| $$  | $$| $$  | $$| $$  | $$| $$  | $$ /$$__  $$| $$ | $$ | $$      
| $$  | $$| $$  | $$|  $$$$$$/| $$$$$$$/|  $$$$$$$| $$ | $$ | $$      
|__/  |__/|__/  |__/ \______/ | $$____/  \_______/|__/ |__/ |__/      
                              | $$                                    
                              | $$                                    
                              |__/                                    
 /$$   /$$           /$$           /$$                                
| $$  /$$/          |__/          | $$                                
| $$ /$$/   /$$$$$$  /$$  /$$$$$$$| $$$$$$$  /$$$$$$$   /$$$$$$       
| $$$$$/   /$$__  $$| $$ /$$_____/| $$__  $$| $$__  $$ |____  $$      
| $$  $$  | $$  \__/| $$|  $$$$$$ | $$  \ $$| $$  \ $$  /$$$$$$$      
| $$\  $$ | $$      | $$ \____  $$| $$  | $$| $$  | $$ /$$__  $$      
| $$ \  $$| $$      | $$ /$$$$$$$/| $$  | $$| $$  | $$|  $$$$$$$      
|__/  \__/|__/      |__/|_______/ |__/  |__/|__/  |__/ \_______/      
                                                                    
`;

const active = ref(false);
const toggleActive = () => {
  //   let state = Flip.getState(".console");
  active.value = !active.value;
  console.log(headerText);
  consoleRef.value.classList.toggle("expanded");

  //   Flip.from(state, {
  //     duration: 2,
  //     ease: "power1.inOut",
  //     // absolute: true,
  //   });
};
onMounted(() => {
  window.addEventListener("keydown", (e) => {
    if (e.key === "`") {
      toggleActive();
    }
  });

  term = new Terminal();
  term.open(consoleRef.value);
});
</script>

<style lang="scss" scoped>
.console {
  position: fixed;
  top: 20px;
  right: 40px;

  border: 1px solid #fff;
  color: #fff;

  font-family: Consolas, monospace;

  height: 40px;
  width: 300px;

  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  flex-direction: column;

  padding: 10px 10px;

  transition: 0.3s;

  pre {
    font: Consolas, monospace;
  }

  .else {
    .entry {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: row;

      input {
        background: transparent;
        border: none;
        color: #fff;
        font-family: Consolas, monospace;
        font-size: 16px;
        width: 100%;
        padding: 0;
        margin: 0;
        outline: none;
      }
    }
  }
}

.expanded {
  height: 100%;
  width: 100%;
  overflow-y: auto;
  padding: 20px;
  font-size: 14px;
  line-height: 1.5;
  background: #141414;
  border-radius: 5px;
  z-index: 9999;
  position: fixed;
  top: 0;
  right: 0;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}
</style>
