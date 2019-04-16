<template>
  <nav class="navigation" :class="alignClass">
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
      <label class="form-check-label asc-radio" for="inlineRadio1">오름차순</label>
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
      <label class="form-check-label desc-radio" for="inlineRadio2">내림차순</label>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Nav",
  data() {
    return {
      selected: "asc"
    };
  },
  computed: {
    alignClass() {
      if (this.selected === "asc") {
        return {
          asc: true
        };
      } else {
        return {
          desc: true
        };
      }
    }
  }
};
</script>

<style lang="less" scoped>
@blue : rgb(82, 123, 255);
.navigation {
  margin: 20px 0 0 10px;
  text-align: center;
  @media (min-width: 480px) {
    position: absolute;
    right: 10%;
    top: 47px;
  }
}
.form-check {
  &-input {
    display: none;
  }
  &-inline {
    margin-right: 0;
    margin-left: 10px;
  }
}
.asc {
  & &-radio {
    color: @blue;
  }
}
.desc {
  & &-radio {
    color: @blue;
  }
}
</style>
