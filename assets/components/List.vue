<template>
  <div class="row">
    <!-- 展示用户列表 -->
    <div
      v-show="info.users.length"
      class="card"
      v-for="user in info.users"
      :key="user.login"
    >
      <a :href="user.html_url" target="_blank">
        <img :src="user.avatar_url" style="width: 100px" />
      </a>
      <p class="card-text">{{ user.login }}</p>
    </div>
    <!-- 展示欢迎词 -->
    <h1 v-show="info.isFirst">欢迎使用！</h1>
    <!-- 展示加载中 -->
    <h1 v-show="info.isLoading">加载中....</h1>
    <!-- 展示错误信息 -->
    <h1 v-show="info.errMsg">{{ info.errMsg }}</h1>
  </div>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      // 欢迎页面
      // 加载中
      // 错误信息
      info: { isFirst: true, isLoading: false, errMsg: "", users: [] },
    };
  },
  mounted() {
    // {isLoading: false,errMsg: "",users: response.data.items, } 这种以对象的形式传递数传递的数据数量不同会丢失isfirst，只有每次传递加上isfirst才可以 于是用es6的...

    this.$bus.$on("updateListData", (dataObj) => {
      // console.log("我成功了！", dataObj);
      // this.info = dataObj;
      // 展开覆盖完成合并  这样写数据的顺序不同也没事是覆盖的
      this.info = { ...this.info, ...dataObj };

      // this.isFirst = isFirst;
      // this.isLoading = isLoading;
      // this.errMsg = errMsg;
      // this.users = users;
    });
  },
  beforeDestroy() {
    this.$bus.$off("updateListData");
  },
};
</script>

<style scoped>
.album {
  min-height: 50rem; /* Can be removed; just added for demo purposes */
  padding-top: 3rem;
  padding-bottom: 3rem;
  background-color: #f7f7f7;
}

.card {
  float: left;
  width: 33.333%;
  padding: 0.75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: 0.75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>