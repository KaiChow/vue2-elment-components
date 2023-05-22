<template>
  <div class="home">
    <ElInput placeholder="请输入内容" />
  </div>
</template>

<script>
// @ is an alias to /src
import ElInput from "@/views/input";
export default {
  name: "HomeView",
  components: { ElInput },
};
</script>
