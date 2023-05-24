<template>
  <div class="home">
    <ElInput v-model="input" placeholder="请输入内容" clearable />
  </div>
</template>

<script>
// @ is an alias to /src
import ElInput from "@/views/input";
export default {
  name: "HomeView",
  components: { ElInput },
  data() {
    return {
      input: "测试",
    };
  },
};
</script>
