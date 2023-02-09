<script>
export default {
  props: ["items", "locations", "activeLocations", "toggleLocation"],
};
</script>

<template>
  <div>
    <p><strong>Filter by location:</strong></p>
    <button
      v-for="filter in locations"
      :key="filter"
      type="button"
      class="btn btn-sm"
      :class="
        activeLocations.includes(filter) ? 'btn-outline-dark' : 'btn-light'
      "
      @click="toggleLocation"
    >
      {{ filter }}
    </button>
  </div>
</template>

<style scoped>
div {
  margin-bottom: 20px;
}

button {
  margin: 0 10px 10px 0;
}
</style>
