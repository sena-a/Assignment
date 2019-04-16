<template>
  <ul id="list-wrapper">
    <template v-for="(item, index) in list">
      <ListItem v-if="(index + 1) % 4 !== 0" :key="item.no" v-bind="item"></ListItem>
      <Ads v-if="(index + 1) % 4 === 0" :key="item.title" v-bind="item"></Ads>
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
      adsList: [],
      bottom: false,
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
      ++this.adsPage;
      return list;
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
      return this.postList;
    },
    getList() {
      this.getPostList()
        .then(() => {
          return this.getAds();
        })
        .then(resolve => {
          this.adsList = this.adsList.concat(resolve);
        })
        .then(() => {
          const post = this.postList;
          for (let i = this.start; i < this.postList.length; i++) {
            if ((i + 1) % 4 === 0) {
              post.splice(i, 0, this.adsList[this.nowAds]);
              ++this.nowAds;
            }
          }
          this.start = this.postList.length;
          this.list = post;
        });
    },
    isScrollBottom() {
      // const isBottom =
      //   document.documentElement.scrollHeight -
      //     document.documentElement.scrollTop ===
      //   document.documentElement.clientHeight;
      const isBottom =
        window.innerHeight + window.scrollY >= document.body.offsetHeight;
      return isBottom;
    }
  },
  created() {
    window.addEventListener("scroll", () => {
      this.bottom = this.isScrollBottom();
    });
    this.$EventBus.$on("setAsc", () => {
      this.align = "asc";
    });
    this.$EventBus.$on("setDesc", () => {
      this.align = "desc";
    });

    this.getList();
  },
  watch: {
    bottom: function(bottom) {
      if (bottom) {
        this.getList();
      }
    },
    align: function(align) {
      this.list = [];
      this.postList = [];
      this.page = 1;
      this.nowAds = 0;
      this.adsPage = 1;
      this.start = 0;
      this.getList();
    },
    cateParams: function(cateParams) {
      this.list = [];
      this.page = 1;
      this.nowAds = 0;
      this.adsPage = 1;
      this.postList = [];
      this.start = 0;
      this.getList();
    }
  }
};
</script>

<style lang="less" scoped>
#list-wrapper {
  padding: 0;
  width: 80%;
  min-width: 270px;
  margin: 0 auto;
}
</style>
