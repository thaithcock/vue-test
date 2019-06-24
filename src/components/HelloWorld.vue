<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <Promised :promise="photoPromise">
      <template v-slot:pending>
        <p>Loading...</p>
      </template>
      <!-- The default scoped slot will be used as the result -->
      <template v-slot="photos">
        <img v-for="photo in photos" v-bind:key="photo.id" v-bind:src="photo.url"/>
      </template>
      <!-- The "rejected" scoped slot will be used if there is an error -->
      <template v-slot:rejected="error">
        <p>Error: {{ error.message }}</p>
      </template>
    </Promised>
  </div>
</template>

<script>
import axios from 'axios';
import { Promised } from 'vue-promised';
import Vue from 'vue';

Vue.component('Promised', Promised);

export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'What up',
      photoPromise: null,
    };
  },
  created() {
    this.photoPromise = axios.get('https://jsonplaceholder.typicode.com/photos?albumId=100')
      .then(response => response.data);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
