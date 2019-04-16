<template>
  <div class="ads-wrapper">
    <p class="ads-sponsor">sponsored</p>
    <div class="ads-container">
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
    padding: 0 15px 10px;
  }
  &-sponsor {
    margin: 10px 3px 7px;
    font-size: 0.7rem;
  }
  &-container {
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
      width: 35%;
      min-width: 140px;
    }
  }
  &-body {
    margin: 14px 10px 5px;
    @media (min-width: 480px) {
      display: inline-block;
      width: 65%;
      height: 90%;
      margin: 0 10px;
    }
  }
  &-title {
    font-size: 1rem;
    margin-top: 10px;
  }
  &-text {
    display: inline-block;
    width: 100%;
    white-space: normal;
    overflow: hidden;
    line-height: 1.2em;
    max-height: 4.8em;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    -webkit-box-orient: vertical;
    -o-text-overflow: ellipsis;
    text-overflow: ellipsis;
    margin: 0;
  }
}
</style>

