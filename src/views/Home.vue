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
