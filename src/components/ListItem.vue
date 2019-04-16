<template>
  <div class="card">
    <div class="card-header">
      <span>{{getCategory}}</span>
      <span>{{no}}</span>
    </div>
    <div class="card-body" @click="toDetail">
      <span>{{email}}</span>
      <span>{{getDate}}</span>
      <h5 class="card-title">{{title}}</h5>
      <p class="card-text">{{contents}}</p>
    </div>
  </div>
</template>
<script>
export default {
  name: "ListItem",
  props: ["category_no", "no", "email", "updated_at", "title", "contents"],
  computed: {
    getCategory() {
      switch (this.category_no) {
        case "1":
          return "apple";
        case "2":
          return "banana";
        case "3":
          return "coconut";
      }
    },
    getDate() {
      const date = this.updated_at.slice(0, 11);
      return date;
    }
  },
  methods: {
    toDetail() {
      this.$EventBus.$emit("toDetail", this.no);
    }
  }
};
</script>

<style lang="less" scoped>
.card {
  margin: 15px 0;
  &-header {
    display: flex;
    justify-content: space-between;
  }
  &-body {
    span:nth-child(1) {
      &::after {
        display: inline-block;
        content: "";
        height: 10px;
        border-left: 1px solid black;
        margin: 0 12px;
      }
    }
  }
  &-title {
    margin: 12px 0 7px;
  }
  &-text {
    display: inline-block;
    width: 100%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
</style>
