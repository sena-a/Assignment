<template>
  <div class="ads-wrapper">
    <div class="ads-img">
      <img
        :src="'http://comento.cafe24.com/public/images/'+img"
        class="card-img"
        :alt="title+'광고 이미지'"
      >
    </div>
    <div class="ads-body">
      <h5 class="ads-title">{{title}}</h5>
      <p class="ads-text">{{contents}}</p>
    </div>
  </div>
</template>
<script>
export default {
  name: "Ads",
  props: ["img", "title", "contents"]
};
</script>
<style lang="less" scoped>
.ads {
  &-wrapper {
    border: 1px solid rgb(226, 226, 226);
    border-radius: 4px;
    @media (min-width: 480px) {
      display: -webkit-box;
      display: -ms-flexbox;
      display: flex;
      -webkit-box-align: center;
      -ms-flex-align: center;
      align-items: center;
    }
  }
  &-img {
    @media (min-width: 480px) {
      display: inline-block;
      width: 30%;
    }
  }
  &-body {
    margin: 14px 10px 5px;
    @media (min-width: 480px) {
      display: inline-block;
      width: 70%;
      margin: 0 10px;
    }
  }
  &-title {
    font-size: 1rem;
  }
  &-text {
    display: inline-block;
    width: 100%;
    white-space: nowrap;
    overflow: hidden;
    -o-text-overflow: ellipsis;
    text-overflow: ellipsis;
    margin: 0;
  }
}
</style>

