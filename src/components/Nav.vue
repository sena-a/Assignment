<template>
  <nav class="navigation">
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
  </nav>
</template>

<script>
export default {
  name: "Nav",
  data() {
    return {
      selected: ""
    };
  }
};
</script>

<style lang="less" scoped>
.navigation {
  margin: 20px 0 0 7px;
}
</style>
