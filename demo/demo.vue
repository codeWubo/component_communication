<template>
  <div id="demo">
    <Add @add="addTask" :styleRed="styleRed"></Add>
    <div class="tasks">
      <Task
        v-for="item in taskArr"
        :key="item.id"
        :task="item"
        @deleteTask="deleteT"
        @check="checkTask"
      ></Task>
    </div>
    <Foot
      ref="foot"
      :finish="finish"
      :all="all"
      @FcheckAll="checkA"
      @deletFinish="deleteFinish"
    ></Foot>
  </div>
</template>

<script>
import Add from "./demo_add.vue";
import Task from "./demo_task.vue";
import Foot from "./demo_footer.vue";
export default {
  name: "demo_app",
  data() {
    return {
      taskArr: [],
      checkTackArr: [],
      finish: 0,
      all: 0,
      styleRed: false,
    };
  },
  // 计算属性
  //勾选即为已完成
  // computed: {
  //   finish() {
  //     return this.checkTackArr.length;
  //   },
  //   all() {
  //     return this.taskArr.length;
  //   },
  // },
  watch: {
    taskArr() {
      this.all = this.taskArr.length;
    },
    checkTackArr() {
      this.finish = this.checkTackArr.length;
    },
  },
  methods: {
    // 添加任务
    addTask(val) {
      if (!val) {
        this.styleRed = true;
        setTimeout(() => {
          this.styleRed = false;
        }, 1000);
        return;
      }
      const task = {
        id: this.guid(),
        name: val,
        ck: false,
      };
      this.taskArr.unshift(task);
      console.log("列表", this.taskArr, this.checkTackArr);
    },
    // 生成唯一标识
    guid() {
      return "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(
        /[xy]/g,
        function (c) {
          var r = (Math.random() * 16) | 0,
            v = c == "x" ? r : (r & 0x3) | 0x8;
          return v.toString(16);
        }
      );
    },
    // 删除任务
    deleteT(obj) {
      this.taskArr.forEach((item, index) => {
        if (item.id === obj.id) {
          this.taskArr.splice(index, 1);
        }
      });
      if (this.checkTackArr.length > 0) {
        console.log("运行了");
        this.checkTackArr.forEach((citem, cindex) => {
          if (obj.id === citem.id) {
            this.checkTackArr.splice(cindex, 1);
          }
        });
      } else {
        this.finish = 0;
        this.$refs.foot.quanbu = false;
      }
      console.log("单个删除", this.taskArr);
    },

    // 选择任务
    checkTask(obj) {
      if (obj.ck) {
        // 选中的时候加入一个数组
        this.checkTackArr.push(obj);
        this.taskArr.forEach((item) => {
          if (item.id === obj.id && !item.ck) {
            item.ck = true;
          }
        });
        console.log("选择后", this.checkTackArr, this.taskArr);
      } else {
        // 取消的时候从数组删除
        this.checkTackArr.forEach((item, index) => {
          if (item.id === obj.id && obj.ck === false) {
            this.checkTackArr.splice(index, 1);
          }
        });
        console.log("取消选择后", this.checkTackArr);
      }
    },

    // 全选任务
    checkA(check) {
      if (check) {
        this.taskArr.forEach((item) => {
          item.ck = true;
        });
        this.checkTackArr = this.taskArr;
        console.log("全选后", this.checkTackArr, this.taskArr);
      } else {
        this.taskArr.forEach((item) => {
          item.ck = false;
        });
        this.checkTackArr = [];
        console.log("取消全选后", this.checkTackArr, this.taskArr);
      }
    },
    // 删除已完成任务
    deleteFinish() {
      // forEach 搭配 splice 会出现langth变化导致的遍历次数问题
      this.taskArr.forEach((item, index) => {
        this.checkTackArr.forEach((Fitem, Findex) => {
          if (item.id === Fitem.id) {
            // copyArr.splice(index, 1);
            // copyChe.splice(Findex, 1);
            delete this.taskArr[index];
            delete this.checkTackArr[Findex];
          }
        });
      });
      console.log("删除之前", this.checkTackArr);
      // Object.values()方法返回一个给定对象自身的所有可枚举属性值的数组
      this.taskArr = Object.values(this.taskArr);
      this.checkTackArr = Object.values(this.checkTackArr);
      this.$refs.foot.quanbu = false;
      console.log("删除后", this.taskArr, this.checkTackArr);
    },
  },
  components: {
    Add,
    Task,
    Foot,
  },
};
</script>

<style lang="less" scoped>
#demo {
  margin: 20px;
  padding: 20px;
  min-width: 400px;
  min-height: 200px;
  border-radius: 30px;
  background-color: #fafafa;
  box-shadow: 2px 2px 3px #9e9e9e, -2px -2px 3px #9e9e9e;
  .tasks {
    padding: 5px 0px;
    height: 200px;
    overflow-y: scroll;
    padding-right: 3px;
    &::-webkit-scrollbar {
      width: 4px;
      border-radius: 2px;
    }
    &::-webkit-scrollbar-thumb {
      border-radius: 2px;
      background-color: #9e9e9e;
    }
    &::-webkit-scrollbar-track {
      // background-color: orange;
    }
  }
}
</style>
