<template>
  <div id="add">
    <!-- 输入框 -->
    <label for="info">输入任务名：</label
    ><input
      id="info"
      type="text"
      :class="{ sred: styleRed }"
      @keydown.enter="add"
      v-model="Task"
      placeholder="按enter确认输入"
    />
  </div>
</template>

<script>
export default {
  name: "demo_add",
  data() {
    return {
      Task: "",
    };
  },
  props: {
    styleRed: {
      type: Boolean,
    },
  },
  methods: {
    // 将组件的task传给父组件
    add() {
      this.$emit("add", this.Task);
      this.Task = "";
    },
  },
};
</script>

<style lang="less" scoped>
#add {
  input {
    border: none;
    outline: none;
    border-bottom: 1px solid gray;
  }

  .sred {
    border-bottom-color: red;
  }
}
</style>
