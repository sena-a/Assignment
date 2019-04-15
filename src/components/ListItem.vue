<template>
  <div class="card">
    <div class="card-header">
      <span>{{category_no}}</span>
      <span>{{no}}</span>
    </div>
    <div class="card-body">
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
  props: ["category_no", "no", "email", "updated_at", "title", "contents"]
};
</script>
<style>
</style>
