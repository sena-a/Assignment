<template>
  <div class="detail-wrapper">
    <button
      type="button"
      class="btn btn-outline-primary btn-sm"
      @click="$EventBus.$emit('toList')"
    >목록으로</button>
    <div class="detail-main">
      <h3>{{title}}</h3>
      <p>{{email}}</p>
      <p>{{date}}</p>
      <div class="detail-content">{{content}}</div>
    </div>
    <div class="list-group detail-comment">
      <h4>Comments</h4>
      <div class="list-group-item detail-comment-each" v-for="item in reply" :key="item.no">
        <div class="d-flex w-100 justify-content-between">
          <small class="text-muted">{{item.email}}</small>
        </div>
        <p class="mb-1">{{item.contents}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Detail",
  props: ["req_no"],
  data() {
    return {
      title: "",
      email: "",
      date: "",
      content: "",
      reply: []
    };
  },
  methods: {
    async getDetail() {
      const {
        data: { detail }
      } = await this.$http.get("http://comento.cafe24.com/detail.php", {
        params: {
          req_no: this.req_no
        }
      });
      const { article, replies } = detail;
      this.title = article.title;
      this.email = article.email;
      this.date = article.updated_at;
      this.content = article.contents;
      this.reply = replies;
    }
  },
  created() {
    this.getDetail();
  }
};
</script>
<style lang="less" scoped>
.detail {
  &-wrapper {
    width: 80%;
    max-width: 1000px;
    margin: 0 auto 50px;
    font-size: 0.9rem;
    button {
      margin-bottom: 50px;
    }
  }
  &-main {
    width: 95%;
    margin: 0 auto;
    h3 {
      margin-bottom: 15px;
    }
    p {
      margin: 5px 0;
    }
  }
  &-content {
    margin: 20px 0 40px;
  }
  &-comment {
    font-size: 0.8rem;
    h4 {
      font-size: 1rem;
    }
    p {
      margin-top: 10px;
    }
    &-each {
      margin-bottom: 12px;
    }
  }
}
</style>

