<template>
  <div class="movie">
    <img v-bind:src="film.poster_path" alt="" />
    <div class="movie-info">
      <h3>{{ film.title }}</h3>
      <span v-bind:class="setVotesClass(film.votes_average)">{{ film.votes_average }}</span>
    </div>
    <div class="overview">
      <h3>Overview</h3>
      {{ film.overview }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, ref } from "vue";
import MovieType from "@/types/movies";

export default defineComponent({
  name: "Movies",
  props: {
    film: {
      required: true,
      type: Object as PropType<MovieType[]>,
    },
  },

  setup() {
    function setVotesClass(vote: number) : string {
      if(vote >= 8) {
        return "green"
      } else if(vote >= 5) {
        return "orange"
      } else {
        return "red"
      }
    }
    return { setVotesClass };
  },
});
</script>
