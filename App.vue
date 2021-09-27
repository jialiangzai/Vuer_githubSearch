<template>
    <div class="container">
      <!-- 搜索 -->
      <Search />
      <!-- list -->
      <List />
    </div>
</template>

<script>
import Search from "./assets/components/Search";
import List from "./assets/components/List";
export default {
  name: "App",
  components: { List , Search },
};
</script>

