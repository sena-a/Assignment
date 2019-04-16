<template>
  <nav class="navigation">
    <button type="button" class="btn btn-outline-primary btn-sm">Fillter</button>
    <div>
      <div class="form-check form-check-inline">
        <input
          class="form-check-input"
          type="radio"
          name="inlineRadioOptions"
          id="inlineRadio1"
          value="asc"
          v-model="selected"
          v-on:click="$EventBus.$emit('setAsc')"
        >
        <label class="form-check-label" for="inlineRadio1">오름차순</label>
      </div>
      <div class="form-check form-check-inline">
        <input
          class="form-check-input"
          type="radio"
          name="inlineRadioOptions"
          id="inlineRadio2"
          value="desc"
          v-model="selected"
          v-on:click="$EventBus.$emit('setDesc')"
        >
        <label class="form-check-label" for="inlineRadio2">내림차순</label>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Nav",
  data() {
    return {
      selected: ""
    };
  },
  methods: {
    alignAsc() {
      this.$emit("asc");
    }
  }
};
</script>

<style scoped>
</style>
