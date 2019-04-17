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
            <button type="button" class="btn btn-primary" data-dismiss="modal" @click="getParams">저장</button>
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
      // 카테고리 목록 상태
      category: [],
      // checkbox 선택한 카테고리 바인딩한 상태
      checkedCate: [],
      // 저장한 카테고리 api parameters 형식으로 str 처리한 상태
      cateParams: ""
    };
  },
  methods: {
    // 카테고리 목록 통신 함수
    async getCategory() {
      const {
        data: { list }
      } = await this.$http.get("http://comento.cafe24.com/category.php");
      this.category = list;
    },
    // 바인딩한 카테고리 배열을 parameters str 형식으로 변환 후 상태에 저장하는 함수
    // 그 후 APP 컴포넌트에 파라미터 상태 전달
    // 저장 버튼 클릭시 실행
    getParams() {
      this.cateParams = this.checkedCate.toString();
      this.$EventBus.$emit("params", this.cateParams);
    },
    // 달기 버튼 클릭시 저장되지 않고 바인딩 중인 배열 내용을 이전 결과로 덮어쓰기.
    modalClose() {
      this.checkedCate = this.cateParams.split(",");
    }
  },
  created() {
    this.getCategory();
  }
};
</script>