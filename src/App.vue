<template>
  <div id="app">
    <div class="header">
      <Fillter v-if="mainPage === 'list'"/>
      <Nav v-if="mainPage === 'list'"/>
    </div>
    <List v-if="mainPage === 'list'" :cateParams="cateParams"/>
    <Detail v-if="mainPage === 'detail'" :req_no="req"/>
  </div>
</template>

<script>
import Nav from "./components/Nav";
import List from "./components/List";
import Detail from "./components/Detail";
import Fillter from "./components/Fillter";

export default {
  name: "App",
  components: {
    Nav,
    List,
    Detail,
    Fillter
  },
  data() {
    return {
      // 리스트 페이지인지 디테일 페이지인지 판별하는 상태
      mainPage: "list",
      // fillter에서 전달되는 리스트 category parameters 저장
      cateParams: "",
      // 개별 글 클릭시 클릭한 글 no 저장 -> Detail 컴포넌트로 전달
      req: 0
    };
  },
  created() {
    // 개별 글 클릭 시 List 컴포넌트에서 해당 게시글 no 받아서 상태로 저장
    // 페이지 상태를 디테일 페이지로 변경
    this.$EventBus.$on("toDetail", number => {
      this.mainPage = "detail";
      this.req = number;
    });
    // 디테일 페이지에서 목록으로 버튼 누르면 리스트 페이지로 변경
    this.$EventBus.$on("toList", () => {
      this.mainPage = "list";
    });
    // 필터 모달에서 저장하기 누르면 선택한 카테고리를 파라미터 형식으로 전달, 현재 컴포넌트에 저장.
    this.$EventBus.$on("params", params => {
      this.cateParams = params;
    });
  }
};
</script>

<style lang="less">
#app {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 0.8rem;
  @media (min-width: 480px) {
    font-size: 0.9rem;
  }
}
.header {
  @media (min-width: 480px) {
    width: 80%;
    max-width: 1000px;
    margin: 0 auto;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: justify;
    -ms-flex-pack: justify;
    justify-content: space-between;
  }
}
.modal {
  width: 100%;
  height: 100%;
  position: fixed;
  background: rgba(0, 0, 0, 0.7);
  &-start {
    width: 40%;
    margin: 0 auto;
    display: block;
    @media (min-width: 480px) {
      width: 70px;
      display: inline-block;
      margin: 0;
    }
  }
}
</style>

