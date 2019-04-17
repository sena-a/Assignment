<template>
  <div class="fillter-wrapper">
    <button
      type="button"
      class="btn btn-outline-primary btn-sm modal-start"
      data-toggle="modal"
      data-target="#Modal"
    >Fillter</button>
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
  </div>
</template>
<script>
export default {
  name: "Fillter",
  data() {
    return {
      category: [],
      checkedCate: [],
      cateParams: ""
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
      this.$EventBus.$emit("params", this.cateParams);
      // return this.cateParams;
    },
    modalClose() {
      this.checkedCate = this.cateParams.split(",");
    }
  },
  created() {
    this.getCategory();
  }
};
</script>