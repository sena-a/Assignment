<template>
  <div class="detail-wrapper">
    <button
      type="button"
      class="btn btn-outline-primary btn-sm"
      @click="$EventBus.$emit('toList')"
    >목록으로</button>
    <h3>{{title}}</h3>
    <p>{{email}}</p>
    <p>{{date}}</p>
    <div>{{content}}</div>
    <div class="list-group">
      <div class="list-group-item" v-for="item in reply" :key="item.no">
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
      this.date = article.update_at;
      this.content = article.contents;
      this.reply = replies;
    }
  },
  created() {
    this.getDetail();
  }
};
</script>
<style>
</style>
