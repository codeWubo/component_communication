<template>
  <div class="task">
    <input type="checkbox" @change="checkbox" v-model="ckk" />
    <span>任务名:{{ task.name }} 任务标识:{{ task.id }}</span>
    <button class="debtn" @click="deleteTask">删除</button>
  </div>
</template>

<script>
export default {
  name: "demo_task",
  data() {
    return {
      ckkbb: false,
    };
  },
  props: {
    task: {
      type: Object,
      required: true,
    },
  },
  // create() {
  //   this.ckkbb = this.ck;
  // },
  computed: {
    ckk: {
      get() {
        return this.task.ck;
      },
      set(value) {
        this.ckkbb = value;
      },
    },
  },
  methods: {
    // 触发父组件的删除事件
    deleteTask() {
      const obj = {
        id: this.task.id,
        name: this.task.name,
        ck: this.ckkbb,
      };
      this.$emit("deleteTask", obj);
    },
    // 选中状态
    checkbox() {
      const obj = {
        id: this.task.id,
        name: this.task.name,
        ck: this.ckkbb,
      };
      this.$emit("check", obj);
    },
  },
};
</script>

<style lang="less">
.task {
  width: 100%;
  height: 40px;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  border-radius: 10px;
  input {
    flex: 1;
  }

  span {
    flex: 8;
  }

  .debtn {
    flex: 1;
    opacity: 0;
    margin: 0px 10px;
    padding: 3px 0px;
    background-color: red;
    border: none;
    outline: none;
    border-radius: 2px;
    color: #efefef;
    font-size: 16px;
    font-weight: 600;
    text-align: center;
    letter-spacing: 5px;
  }

  &:hover {
    background-color: #efefef;
  }

  &:hover .debtn {
    opacity: 1;
  }
}
</style>
