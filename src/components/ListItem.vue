<script>
const DEFAULT_IMAGE_PLACEHOLDER =
  "https://via.placeholder.com/50/000000/FFFFFF";

export default {
  props: ["imgSrc", "name", "description", "date", "searchTerm"],
  methods: {
    replaceByDefault(e) {
      e.target.src = DEFAULT_IMAGE_PLACEHOLDER;
    },
    highlightString(str) {
      var reg = new RegExp(this.$props.searchTerm, "gi");

      return str?.replace(reg, function (str) {
        return (
          '<span style="background-color: navajowhite; font-weight: inherit;">' +
          str +
          "</span>"
        );
      });
    },
  },
  computed: {
    userNavigatorLang() {
      return navigator?.language;
    },
    userAvatar() {
      const src = this.$props.imgSrc || DEFAULT_IMAGE_PLACEHOLDER;
      return src;
    },
  },
};
</script>

<template>
  <li>
    <img :src="userAvatar" @error="replaceByDefault" class="rounded-circle" />
    <div>
      <h2>
        <strong v-html="highlightString(this.name.split(' ')[0])"></strong>,
        <span v-html="highlightString(this.name.split(' ')[1])"></span>
      </h2>
      <p>
        <span v-html="highlightString(this.description)"></span>
        <small>{{
          new Intl.DateTimeFormat(userNavigatorLang).format(new Date(this.date))
        }}</small>
      </p>
    </div>
  </li>
</template>

<style scoped>
li {
  display: flex;
  gap: 16px;
  width: 100%;
  padding: 10px;
  margin-bottom: -1px;
  border: 1px solid lightgray;
}

img {
  flex: 0 0 1;
  width: 50px;
  height: 50px;
}

h2 {
  margin: 0;
  font-size: 24px;
}

small {
  display: block;
}
</style>
