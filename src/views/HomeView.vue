<template>
  <div class="home">
    <Eltag closable>测试1</Eltag>
    <Eltag closable>测试2</Eltag>
    <Eltag closable>测试3</Eltag>
    <Eltag closable>测试4</Eltag>
  </div>
</template>

<script>
// @ is an alias to /src
import Eltag from "@/views/tag";
export default {
  name: "HomeView",
  components: { Eltag },
  data() {
    return {
      input: "测试",
    };
  },
};
</script>
