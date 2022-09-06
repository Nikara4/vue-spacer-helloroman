<template>
  <div class="wrapper">
    <div class="search">
      <!-- eslint-disable-next-line vuejs-accessibility/label-has-for -->
      <label htmlFor="search">
        Search</label>
      <input id="search" name="search" v-model="searchValue" @input="handleInput" />
    </div>

    <div class="images">
      <ul>
        <li
          v-for="item in results"
          :key="item.data[0].nasa_id"
        >
          <p>{{ item.data[0].description }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const NASA_API = 'https://images-api.nasa.gov/search';

export default {
  name: 'HomeView',
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    // eslint-disable-next-line func-names
    handleInput: debounce(function () {
      axios.get(`${NASA_API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          console.log(this.results);
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>
s
<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@200;300;400&display=swap');

.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding: 30px;
  width: 100%;

  & .search {
    display: flex;
    flex-direction: column;
    width: 250px;
  }

  & label {
    font-family: 'Montserrat', 'sans-serif';
    font-weight: 400;
  }

  & input {
    height: 30px;
    border: 0;
    border-bottom: 1px solid black;
  }
}
</style>
