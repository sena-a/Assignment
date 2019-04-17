<template>
  <ul id="list-wrapper">
    <template v-for="(item, index) in list">
      <!-- 4번째 인덱스가 게시글 컴포넌트로 출력 -->
      <ListItem v-if="(index + 1) % 4 !== 0" :key="item.no" v-bind="item"></ListItem>
      <!-- 4번째 인덱스면 광고글 컴포넌트로 출력 -->
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
      // 게시글 목록
      postList: [],
      // 포스트 리스트 요청 page 파라미터 기억
      page: 1,
      // 광고글 목록
      adsList: [],
      // 삽입할 차례인 광고 아이템 인덱스 기억
      nowAds: 0,
      // 광고 리스트 요청 page 파라미터 기억
      adsPage: 1,
      // 순회를 시작할 포스트 리스트 아이템 인덱스 기억
      start: 0,
      // 실제 화면에 출력할 최종 리스트
      list: [],
      // 스크롤 최하단 여부 판단 상태
      bottom: false,
      // 정렬 상태
      align: "asc"
    };
  },
  props: ["cateParams"],
  methods: {
    // 광고 리스트 요청 함수
    async getAds() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/ads.php", {
        params: {
          page: this.adsPage,
          limit: 4
        }
      });
      // 다음 불러와야할 광고 페이지 저장
      ++this.adsPage;
      return list;
    },
    // 게시글 리스트 요청 함수
    async getPostList() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/request.php", {
        params: {
          page: this.page,
          ord: this.align,
          // 필터에서 아무것도 선택되지 않았을 때 기본 값 -> 카테고리 전체
          category: this.cateParams === "" ? null : this.cateParams
        }
      });
      // 게시글 리스트에 합체
      this.postList = this.postList.concat(list);
      // 다음 불러와야할 게시글 페이지 저장
      ++this.page;
      return this.postList;
    },
    getList() {
      // 게시글 리스트 저장
      this.getPostList()
        .then(() => {
          // 불러온 광고 리스트 확보
          return this.getAds();
        })
        .then(resolve => {
          // 광고 리스트 저장
          this.adsList = this.adsList.concat(resolve);
        })
        .then(() => {
          const post = this.postList;
          // 순회 시작 인덱스부터 게시글 리스트 마지막 인덱스까지 순회
          for (let i = this.start; i < this.postList.length; i++) {
            // 순회 중 3, 7, 11... 인덱스에는 광고 아이템 삽입
            if ((i + 1) % 4 === 0) {
              post.splice(i, 0, this.adsList[this.nowAds]);
              // 다음 삽입할 광고 아이템 인덱스 증가
              ++this.nowAds;
            }
          }
          this.start = this.postList.length;
          // 삽입한 최종 리스트 저장
          this.list = post;
        });
    },
    isScrollBottom() {
      // 스크롤 최하단일 시 true, 아닐 시 false
      const isBottom =
        window.innerHeight + window.scrollY >= document.body.offsetHeight;
      return isBottom;
    }
  },
  created() {
    // 스크롤 이벤트 발생 시 최하단인지 아닌지 상태 저장
    window.addEventListener("scroll", () => {
      this.bottom = this.isScrollBottom();
    });
    // 정렬 상태 변경 시 반응
    this.$EventBus.$on("setAsc", () => {
      this.align = "asc";
    });
    this.$EventBus.$on("setDesc", () => {
      this.align = "desc";
    });

    this.getList();
  },
  watch: {
    // 최하단일 시 목록 추가
    bottom: function(bottom) {
      if (bottom) {
        this.getList();
      }
    },
    // 정렬 내용 변경 시 목록 초기화 후 다시 목록 셋팅
    align: function(align) {
      this.list = [];
      this.postList = [];
      this.page = 1;
      this.nowAds = 0;
      this.adsPage = 1;
      this.start = 0;
      this.getList();
    },
    // 필터 조건 변경 시 목록 초기화 후 다시 목록 셋팅
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
