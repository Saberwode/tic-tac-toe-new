<template>
  <div id="app">
    <block></block>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import block from "@/components/block.vue";

export default {
  name: "app",
  data() {
    return {};
  },
  components: {
    block,
  },
};
</script>

<style scoped>
.box {
  background: #fff;
  margin: 0 auto;
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  float: left;
  border-left: 1px solid black;
  border-bottom: 1px solid black;
}

.body {
  width: 303px;
  height: 100px;
  margin: 0 auto;
  display: block;
  border-right: 1px solid black;
  border-top: 1px solid black;
}
</style>