<template>
  <div id="app">
    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-outline-primary btn-sm modal-open"
      data-toggle="modal"
      data-target="#Modal"
    >Fillter</button>
    <!-- Modal -->
    <div
      class="modal fade"
      id="Modal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">필터</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
              @click="modalClose"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="form-check form-check-inline" v-for="item in category" :key="item.no">
              <input
                class="form-check-input"
                type="checkbox"
                :id="item.name"
                :value="item.no"
                v-model="checkedCate"
              >
              <label class="form-check-label" :for="item.name">{{item.name}}</label>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-primary"
              data-dismiss="modal"
              @click="getParams"
            >Save changes</button>
          </div>
        </div>
      </div>
    </div>
    <Nav/>
    <List v-if="mainPage === 'list'" :cateParams="cateParams"/>
    <Detail v-if="mainPage === 'detail'" :req_no="req"/>
  </div>
</template>

<script>
import Nav from "./components/Nav";
import List from "./components/List";
import Detail from "./components/Detail";

export default {
  name: "App",
  components: {
    Nav,
    List,
    Detail
  },
  data() {
    return {
      category: [],
      checkedCate: [],
      cateParams: "",
      mainPage: "list",
      req: 0
    };
  },
  methods: {
    async getCategory() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/category.php");
      this.category = list;
    },
    getParams() {
      this.cateParams = this.checkedCate.toString();
      return this.cateParams;
    },
    modalClose() {
      this.checkedCate = this.cateParams.split("");
    }
  },
  created() {
    this.getCategory();
    this.$EventBus.$on("toDetail", number => {
      this.mainPage = "detail";
      this.req = number;
    });
    this.$EventBus.$on("toList", () => {
      this.mainPage = "list";
    });
  }
};
</script>

<style lang="less">
#app {
  min-width: 300px;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 0.8rem;
}
.modal {
  width: 100%;
  height: 100%;
  position: fixed;
  background: rgba(0, 0, 0, 0.7);
  &-open {
    width: 50%;
  }
}
</style>

