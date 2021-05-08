<template>
  <div class="main">
    <button @click="show = !show">切り替え</button>
    <br>
    <button @click="addList">リスト追加</button>

    <ul style="width:200px; margin: auto;">
      <transition-group name="fade">
        <li
          style="cursor: pointer;"
          v-for="(number, index) in numbers"
          :key="number"
          @click="remove(index)"
        >{{ number }}
        </li>
      </transition-group>
    </ul>

    <br><br>
    <transition
      :css="false"
      @before-enter="beforeEnter"
      @enter="enter"
      @leave="leave"
    >
      <div class="circle" v-if="show"></div>
    </transition>
    <br>
    <transition name="slide" type="animation" appear>
      <p v-if="show">bye</p>
      <p v-else></p>
    </transition>
    <button @click="myComponent = 'CompA'">A</button>
    <button @click="myComponent = 'CompB'">B</button>
    <transition name="fade" mode="out-in">
      <component :is="myComponent"> </component>
    </transition>
    <br>
    <transition name="fade" mode="out-in">
      <p v-if="show" key="bye">またね</p>
      <p v-else key="hello"> こんにちは</p>
    </transition>
    <transition
      enter-active-class="animate__animated animate__bounce"
      leave-active-class="animate__animated animate__flash"
    >
      <p v-if="show">hello</p>
    </transition>
  </div>

</template>

<script>
import CompA from "./components/ComponentA.vue"
import CompB from "./components/ComponentB.vue"
export default {
  components: {
    CompA,
    CompB
  },
  data() {
    return {
      numbers: [0, 1, 2],
      nextNumber: 3,
      show: true,
      myComponent: CompA
    };
  },
  methods: {
    randomIndex() {
      return Math.floor(Math.random() * this.numbers.length);
    },
    addList() {
      this.numbers.splice(this.randomIndex(), 0, this.nextNumber);
      this.nextNumber += 1;
    },
    remove(index) {
      this.numbers.splice(index, 1);
    },

    beforeEnter(el){
      el.style.transform = 'scale(0)'
    },
    enter(el,done){
      let scale = 0;
      const interval = setInterval(() => {
        el.style.transform = `scale(${scale})`;
        scale += 0.1;
        if (scale > 1) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    leave(el,done){
      let scale = 1;
      const interval = setInterval(() => {
        el.style.transform = `scale(${scale})`;
        scale -= 0.1;
        if( scale < 0) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    // leaveCancelled(el){}
  }
};
</script>

<style scoped>

.fade-move{
  transition: transform 1s;
}
.circle {
  width: 200px;
  height: 200px;
  margin: auto;
  border-radius: 100px;
  background: chocolate;
}
.fade-enter{
  opacity: 0;
}
.fade-enter-active{
  transition: opacity 1s;
}
.fade-enter-to{
  opacity: 1;
}
.fade-leave{
    opacity: 1;
}
.fade-leave-active{
  transition: opacity 1s;
  position: absolute;
  width: 200px;
}
.fade-leave-to{
      opacity: 0;
}


.slide-enter,
.slide-leave-to{
    opacity: 0;
}
.slide-enter-active{
  animation: slide-in 0.5s;
  transition: opacity 1s;
}
.slide-leave-active{
  animation: slide-in 0.5s reverse;
  transition: opacity 1s;
}

@keyframes slide-in {
  from{
    transform: translate(100px);
  }
  to{
    transform: translate(0);
  }
}
.main {
  width: 70%;
  margin: auto;
  padding-top: 5rem;
  text-align: center;
}
</style>