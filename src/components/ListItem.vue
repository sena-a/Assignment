<template>
  <div class="card">
    <div class="card-header">
      <span>{{getCategory}}</span>
      <span>{{no}}</span>
    </div>
    <div class="card-body" @click="toDetail">
      <span>{{email}}</span>
      <span>{{updated_at}}</span>
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
    }
  },
  methods: {
    toDetail() {
      this.$EventBus.$emit("toDetail", this.no);
    }
  }
};
</script>
<style>
</style>
