<script>
import ListItem from "./components/ListItem.vue";
import InputSearch from "./components/InputSearch.vue";
import LocationsList from "./components/LocationsList.vue";

export default {
  name: "App",
  components: {
    ListItem,
    InputSearch,
    LocationsList,
  },
  data() {
    return {
      items: [],
      searchTerm: "",
      locations: [],
      activeLocations: [],
      filteredItems: [],
    };
  },
  methods: {
    changeInputValue(event) {
      this.searchTerm = event.target.value.trim();
      this.filterItems();
    },

    toggleLocation(event) {
      const value = event.target.textContent.trim();
      const locationsClone = [...this.activeLocations];
      const index = locationsClone.indexOf(value);

      if (index === -1) {
        this.activeLocations = [...locationsClone, value];
      } else {
        this.activeLocations.splice(index, 1);
      }

      this.filterItems();
    },

    filterItems() {
      let filtered = [...this.items];

      // Building a regular expression to find matches instead of using includes as the
      // includes method wouldn't work because of the case insensitiveness.
      const buildRegex = (value) => new RegExp(`(${value})+`, "gi");

      // Apply the search term filter
      if (this.searchTerm) {
        filtered = filtered.filter((item) => {
          const searchTermRegex = buildRegex(this.searchTerm);
          return (
            item["First name"]?.match(searchTermRegex) ||
            item["Last name"]?.match(searchTermRegex) ||
            item["Job title"]?.match(searchTermRegex)
          );
        });
      }

      // Apply the locations filter
      if (this.activeLocations.length) {
        filtered = filtered.filter((item) => {
          return this.activeLocations.includes(item.Location);
        });
      }

      this.filteredItems = filtered;
    },

    setData(data) {
      this.items = data;
      const uniqueLocations = [
        ...new Set(data.map((location) => location.Location)),
      ];

      this.locations = [...uniqueLocations];
      this.activeLocations = [...uniqueLocations];
    },
  },
  mounted() {
    fetch(
      "https://gist.githubusercontent.com/tonytlwu/a4cc0f235c75da3cb6a419011710b28c/raw/1d8ac5aa22fb893c5559a5ba069b75e0c6f463b3/sample.json"
    )
      .then((response) => response.json())
      .then((data) => {
        this.setData(data);
        this.filterItems();
      });
  },
};
</script>

<template>
  <div id="app">
    <main>
      <!-- Search input -->
      <InputSearch :inputValue="changeInputValue" />
      <!-- Location filters -->
      <LocationsList
        :items="items"
        :locations="locations"
        :activeLocations="activeLocations"
        :toggleLocation="toggleLocation"
      />
      <!-- List -->
      <ul>
        <ListItem
          v-for="item in filteredItems"
          :key="item['Date of birth'] + '-' + item['First name']"
          :img-src="item.Image?.url || item.Image"
          :name="item['Last name'] + ' ' + item['First name']"
          :description="item['Job title']"
          :date="item['Date of birth']"
          :searchTerm="searchTerm"
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
