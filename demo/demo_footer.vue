<template>
  <div id="footer">
    <input type="checkbox" @change="checkAll" v-model="quanbu" />
    <span>已完成{{ finish }}/全部{{ all }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quanbu: false,
    };
  },
  props: {
    finish: {
      type: Number,
      required: true,
    },
    all: {
      type: Number,
      required: true,
    },
  },
  methods: {
    checkAll() {
      // 触发父组件的全选
      this.$emit("FcheckAll", this.quanbu);
    },
  },
};
</script>
