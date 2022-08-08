<template>
  <div id="footer">
    <div class="f_left">
      <input type="checkbox" @change="checkAll" v-model="quanbu" />
      <span>已完成{{ finish }}/全部{{ all }}</span>
    </div>
    <div class="f_right">
      <button class="debtn" @click="deleteFinish" v-if="finish > 0">
        删除已完成任务
      </button>
    </div>
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
  watch:{  
    finish(newValue){
      if(newValue === this.all && newValue != 0) {
        this.quanbu = true;
      } else {
        this.quanbu = false;
      }
    }
  },
  methods: {
    checkAll() {
      console.log(this.quanbu);
      // 触发父组件的全选
      this.$emit("FcheckAll", this.quanbu);
    },
    deleteFinish() {
      // 删除已勾选任务
      this.$emit("deletFinish");
    },
  },
};
</script>

<style lang="less" scoped>
#footer {
  height: 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  .f_right {
    .debtn {
      margin: 0px 10px;
      padding: 8px 5px;
      background-color: red;
      border: none;
      outline: none;
      border-radius: 2px;
      color: #efefef;
      font-size: 12px;
      font-weight: 600;
      text-align: center;
    }
  }
}
</style>
