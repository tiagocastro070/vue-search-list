<script>
import ListItem from "./components/ListItem.vue";

export default {
  name: "App",
  components: {
    ListItem,
  },
  data() {
    return {
      items: [],
    };
  },
  mounted() {
    fetch(
      "https://gist.githubusercontent.com/tonytlwu/a4cc0f235c75da3cb6a419011710b28c/raw/1d8ac5aa22fb893c5559a5ba069b75e0c6f463b3/sample.json"
    )
      .then((response) => response.json())
      .then((data) => {
        this.items = data;
      });
  },
};
</script>

<template>
  <div id="app">
    <main>
      <!-- List -->
      <ul>
        <ListItem
          v-for="item in items"
          :key="item['Date of birth'] + '-' + item['First name']"
          :img-src="item.Image?.url || item.Image"
          :name="item['Last name'] + ' ' + item['First name']"
          :description="item['Job title']"
          :date="item['Date of birth']"
        />
      </ul>
    </main>
  </div>
</template>

<style scoped>
#app {
  max-width: 600px;
  margin: 0 auto;
}

ul {
  padding: 0;
  margin: 0;
  list-style: none;
}
</style>
