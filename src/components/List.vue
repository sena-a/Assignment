<template>
  <ul>
    <!-- <template v-for="(item, index) in list">
      <ListItem v-if="(index + 1) % 4 !== 0" :key="item.no" v-bind="item"></ListItem>
      <Ads v-if="(index + 1) % 4 === 0" :key="item.title" v-bind="item"></Ads>
    </template>-->
    <template v-for="item in postList">
      <ListItem :key="item.no" v-bind="item"></ListItem>
    </template>
  </ul>
</template>

<script>
import ListItem from "./ListItem";
import Ads from "./Ads";
export default {
  name: "List",
  components: {
    ListItem,
    Ads
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
      list: [],
      align: "asc"
    };
  },
  props: ["cateParams"],
  methods: {
    async getAds() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/ads.php", {
        params: {
          page: this.adsPage,
          limit: 4
        }
      });
      console.log(list);
      this.adsList = this.adsList.concat(list);
      ++this.adsPage;
      this.getList();
    },
    async getPostList() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/request.php", {
        params: {
          page: this.page,
          ord: this.align,
          category: this.cateParams === "" ? "1,2,3" : this.cateParams
        }
      });
      this.postList = this.postList.concat(list);
      ++this.page;
    },
    getList() {
      const newList = [];
      for (let i = this.start; i < this.postList.length; i++) {
        if (i === this.nextAds) {
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
    // this.getAds();
    window.addEventListener("scroll", () => {
      this.bottom = this.isScrollBottom();
    });
    this.$EventBus.$on("setAsc", () => {
      this.align = "asc";
    });
    this.$EventBus.$on("setDesc", () => {
      this.align = "desc";
    });

    this.getPostList();
  },
  watch: {
    bottom: function(bottom) {
      if (bottom) {
        // this.getAds();
        this.getPostList();
      }
    },
    align: function(align) {
      // this.getAds();
      this.postList = [];
      this.page = 1;
      this.getPostList();
    },
    cateParams: function(cateParams) {
      this.postList = [];
      this.page = 1;
      this.getPostList();
    }
  }
};
</script>

<style>
</style>
