<template>
  
  <main>
    <div class="sidebar-left">
      <SidebarLeft />
    </div>
    <div class="main-content">
      <UpperMiddle />
      <LowerMiddle />
    </div>
    
    <div class="sidebar-right">
      <UpperRight />
      <LowerRight />
    </div>
  </main>
</template>

<script lang="ts">
import SidebarLeft from './components/SidebarLeft.vue';
import UpperMiddle from './components/UpperMiddle.vue';
import LowerMiddle from './components/LowerMiddle.vue';
import UpperRight from './components/UpperRight.vue';
import LowerRight from './components/LowerRight.vue';

export default {
  name: 'App',
  components: {
    SidebarLeft,
    UpperMiddle,
    LowerMiddle,
    UpperRight,
    LowerRight
  }
}
</script>

<style>
/* 设置适当的样式来布局页面 */
#app {
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  justify-content: center; /* 水平居中 */
  height: 95vh; /* 使用 100vh 设置高度为视口高度 */
  width: 100vw; /* 使用 100vw 设置宽度为视口宽度 */
}

main {
  display: flex;
  flex: 1;
}

.sidebar-left {
  flex: 4;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 2;
  display: flex;
  flex-direction: column;
}

.sidebar-right {
  flex: 4;
  display: flex;
  flex-direction: column;
}
</style>
