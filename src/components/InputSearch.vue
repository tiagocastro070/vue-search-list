<script>
export default {
  props: ["inputValue"],
};
</script>

<template>
  <div>
    <input
      type="text"
      class="form-control"
      placeholder="Type to search..."
      v-on:input="inputValue"
    />
  </div>
</template>

<style scoped>
div {
  margin: 20px 0;
}

input {
  display: block;
  width: 100%;
}
</style>
