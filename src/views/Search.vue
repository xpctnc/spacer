<template>
  <div class="container">
    <div class="search">
      <label for="search-input">Search</label>
      <input
        type="text"
        id="search-input"
        name="search"
        v-model="searchValue"
        @input="handleInput"
      >
    </div>
    <ul>
      <li v-for="item in results" :key="item.data[0].nasa_id">
        <p> {{ item.data[0].title }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'; /* eslint-disable */

import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov';

export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
      results: []
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
        .then(res => {
          this.results = res.data.collection.items;
          console.log(this.results)
        })
        .catch(error => console.log(error))
    }, 500),
  },
};

</script>

<style lang="scss" scoped>
  div.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 30px;
  }

  div.search {
    width: 300px;
    display: flex;
    flex-direction: column;
    align-items: center;

    label {
      font-family: Montserrat, sans-serif;
    }
    input {
      width: 250px;
      border: none;
      border-bottom: 1px solid black
    }
  }
</style>
