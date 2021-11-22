<template>
  <header>
    <form id="form" @submit.prevent="search()">
      <input
        type="text"
        id="searchTerm"
        name="searchTerm"
        class="search"
        placeholder="Search"
        v-model="searchTerm"
      />
    </form>
  </header>
  <main id="main">
    <Movie
      class="movie"
      v-for="movie in movies"
      :key="movie.id"
      :film="movie"
    />
  </main>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import Movie from "./components/Movie.vue";
import MovieType from "@/types/movies";

export default defineComponent({
  name: "App",
  components: { Movie },
  setup() {
    const movies = ref<MovieType[]>([]);
    const searchTerm = ref("");

    onMounted(() => {
      loadMovies(
        "https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=3fd2be6f0c70a2a598f084ddfb75487c&page=1"
      );
    });

    function loadMovies(url: string) {
      fetch(url)
        .then((response) => {
          if (response.ok) {
            return response.json();
          } else {
            console.log(response.statusText);
          }
        })
        .then((data) => {
          const movieData = data.results;
          const films: MovieType[] = [];
          for (var i = 0; i < movieData.length; i++) {
            const film = movieData[i];
            films.push({
              id: film.id,
              title: film.title,
              poster_path:
                "https://image.tmdb.org/t/p/w1280/" + film.poster_path,
              votes_average: film.vote_average,
              overview: film.overview,
            });
          }
          movies.value = films;
        });
    }

    function search() {
      loadMovies(
        "https://api.themoviedb.org/3/search/movie?api_key=3fd2be6f0c70a2a598f084ddfb75487c&query=" +
          searchTerm.value
      );
    }
    return { movies, search, searchTerm };
  },
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;400&display=swap");

:root {
  --primary-color: #22254b;
  --secondary-color: #373b69;
}

* {
  box-sizing: border-box;
}

body {
  background-color: var(--primary-color);
  font-family: "Poppins", sans-serif;
  margin: 0;
}

header {
  padding: 1rem;
  display: flex;
  justify-content: flex-end;
  background-color: var(--secondary-color);
}

.search {
  background-color: transparent;
  border: 2px solid var(--primary-color);
  border-radius: 50px;
  font-family: inherit;
  font-size: 1rem;
  padding: 0.5rem 1rem;
  color: #fff;
}

.search::placeholder {
  color: #7378c5;
}

.search:focus {
  outline: none;
  background-color: var(--primary-color);
}

main {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.movie {
  width: 300px;
  margin: 1rem;
  background-color: var(--secondary-color);
  box-shadow: 0 4px 5px rgba(0, 0, 0, 0.2);
  position: relative;
  overflow: hidden;
  border-radius: 3px;
}

.movie img {
  width: 100%;
}

.movie-info {
  color: #eee;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 1rem 1rem;
  letter-spacing: 0.5px;
}

.movie-info h3 {
  margin-top: 0;
}

.movie-info span {
  background-color: var(--primary-color);
  padding: 0.25rem 0.5rem;
  border-radius: 3px;
  font-weight: bold;
}

.movie-info span.green {
  color: lightgreen;
}

.movie-info span.orange {
  color: orange;
}

.movie-info span.red {
  color: red;
}

.overview {
  background-color: #fff;
  padding: 2rem;
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  max-height: 100%;
  transform: translateY(101%);
  overflow-y: auto;
  transition: transform 0.3s ease-in;
}

.movie:hover .overview {
  transform: translateY(0);
}
</style>
