<template>
  <ul>
    <template v-for="(item, index) in list">
      <ListItem v-if="(index + 1) % 4 !== 0" :key="item.no" v-bind="item"></ListItem>
    </template>
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
      postList: [],
      page: 1,
      bottom: false,
      adsList: [],
      nextAds: 3,
      nowAds: 0,
      adsPage: 1,
      start: 0,
      list: []
    };
  },
  methods: {
    async getAds() {
      const pageIndex = (this.page - 1) % 3;
      if (pageIndex === 0) {
        // 광고 가져오기
        const {
          data: { list }
        } = await this.$http.get("http://comento.cafe24.com/ads.php", {
          params: {
            page: this.adsPage
          }
        });
        this.adsList = this.adsList.concat(list);
        ++this.adsPage;
      }
    },
    async getPostList() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/request.php", {
        params: {
          page: this.page
        }
      });
      this.postList = this.postList.concat(list);
      ++this.page;
      this.getList();
    },
    getList() {
      const newList = [];
      for (let i = this.start; i < this.postList.length; i++) {
        if ((i + 1) % 4 === 0) {
          console.log(i);
          newList.push(this.adsList[this.nowAds]);
          ++this.nowAds;
          this.nextAds += 3;
        }
        newList.push(this.postList[i]);
      }
      this.start = this.postList.length;
      this.list = this.list.concat(newList);
      console.log(this.list);
    },
    isScrollBottom() {
      const isBottom =
        document.documentElement.scrollHeight -
          document.documentElement.scrollTop ===
        document.documentElement.clientHeight;
      return isBottom;
    }
  },
  created() {
    window.addEventListener("scroll", () => {
      this.bottom = this.isScrollBottom();
    });
    this.getAds();
    this.getPostList();
  },
  watch: {
    bottom: function(bottom) {
      if (bottom) {
        this.getAds();
        this.getPostList();
      }
    }
  }
};
</script>

<style>
</style>
