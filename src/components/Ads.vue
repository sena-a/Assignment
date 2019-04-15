<template>
  <div class="card mb-3" style="max-width: 540px;">
    <div class="row no-gutters">
      <div class="col-md-4">
        <img :src="'http://comento.cafe24.com/public/images/'+img" class="card-img" alt="...">
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">{{title}}</h5>
          <p class="card-text">{{contents}}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Ads",
  props: ["img", "title", "contents"]
};
</script>
<style>
</style>
