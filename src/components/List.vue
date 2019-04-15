<template>
  <ul>
    <ListItem v-for="item in list" :key="item.no" v-bind="item"></ListItem>
  </ul>
</template>

<script>
import ListItem from "./ListItem";
export default {
  name: "List",
  components: {
    ListItem
  },
  data() {
    return {
      list: [],
      page: 1,
      bottom: false
    };
  },
  methods: {
    async getList() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/request.php", {
        params: {
          page: this.page
        }
      });
      this.list = this.list.concat(list);
      ++this.page;
    },
    isScrollBottom() {
      const isBottom =
        document.documentElement.scrollHeight -
          document.documentElement.scrollTop ===
        document.documentElement.clientHeight;
      return isBottom;
    }
  },
  watch: {
    bottom: function(bottom) {
      if (bottom) {
        this.getList();
      }
    }
  },
  created() {
    window.addEventListener("scroll", () => {
      this.bottom = this.isScrollBottom();
    });
    this.getList();
  }
};
</script>

<style>
</style>
