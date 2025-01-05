<script setup lang="ts">
import MyHello from './components/MyHello.vue'
</script>

<style>
body {
  background: #fff;
}
h1,
p,
div {
  color: #000;
}
</style>

<template>
  <div><h1>hello world</h1></div>
  <MyHello title="Кроссы 1" :price="10000" :isAdded="true">
    <button>Hello button</button>
  </MyHello>
  <MyHello title="Кроссы 2" :price="20000">
    <h4>shak</h4>
  </MyHello>
  <MyHello title="Кроссы 3" :price="30000"></MyHello>
  <MyHello title="Кроссы 4" :price="40000"></MyHello>
</template>
