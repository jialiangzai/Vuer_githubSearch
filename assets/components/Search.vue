<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="keyWord"
      />&nbsp;
      <button @click="searchUsers">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      keyWord: "",
    };
  },
  methods: {
    searchUsers() {
      this.$bus.$emit("updateListData", {
        isFirst: false,
        isLoading: true,
        errMsg: "",
        users: [],
      });
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
        (response) => {
          // response.data返回一个数组
          //   console.log("请求成功了！" + response.data.items);
          //   this.$bus.$emit("updateListData", response.data.items);
          // isFirst:false, 只会响应一次 所以去掉 错误信息未来可能有不能去
          // {isLoading: false,errMsg: "",users: response.data.items, } 这种以对象的形式传递数会丢失isfirst，传递的数据数量不同，只有每次传递加上isfirst才可以 于是用es6的合并...

          this.$bus.$emit("updateListData", {
            isLoading: false,
            errMsg: "",
            users: response.data.items,
          });
        },
        (error) => {
          console.log("请求失败了！" + error.message);
          this.$bus.$emit("updateListData", {
            isLoading: false,
            errMsg: error.message,
            // 这里一定要把信息清空为下次搜索做铺垫
            users: [],
          });
        }
      );
    },
  },
};
</script>
