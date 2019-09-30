<template>
  <div class="searchWrapper">
    <div class="search">
      <label for="search">Search</label>
      <input id="search" name="search" v-model="searchValue" @input="handleInput" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import debounce from "lodash.debounce";
import Claim from "@/components/Claim";

const API = "https://images-api.nasa.gov/search";

export default {
  name: "Search",
  components: {
    Claim
  },
  data() {
    return {
      searchValue: "",
      results: []
    };
  },
  methods: {
    handleInput: debounce(function() {
      axios
        .get(`${API}?q=${this.searchValue}&media_type=image`)
        .then(response => {
          this.results = response.data.collection.items;
        })
        .catch(err => console.log(err));
    }, 500)
  }
};
</script>

<style scoped>
.searchWrapper {
  display: flex;
  flex-direction: column;
  width: 250px;
}

input {
  height: 30px;
  border: none;
  border-bottom: 1px solid black;
}
</style>