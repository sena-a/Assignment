<template>
  <nav class="navigation">
    <button type="button" class="btn btn-outline-primary btn-sm">Fillter</button>
    <div class="btn-group btn-group-toggle" data-toggle="buttons">
      <label class="btn btn-outline-primary btn-sm active">
        <input type="radio" name="options" id="option1" autocomplete="off" checked> 오름차순
      </label>
      <label class="btn btn-outline-primary btn-sm">
        <input type="radio" name="options" id="option2" autocomplete="off"> 내림차순
      </label>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Nav"
};
</script>

<style scoped>
</style>
