<script setup>
import { ref } from "vue";
import axios from "axios";

const apiKey = import.meta.env.VITE_API_KEY;

let content = ref(null);
let trailer = ref(null);

async function getContent() {
  const id = movies.value;
  axios
    .get(
      `https://api.themoviedb.org/3/movie/${id}?api_key=${apiKey}&language=en-US&append_to_response=videos`
    )
    .then(function (Information) {
      content.value = Information.data;
      trailer.value = content._rawValue.videos.results.filter((trailer) => {
        return trailer.type === "Trailer";
      });
    });
}
</script>

<template>
  <h1 id="title"><u>Movie Selector</u></h1>
  <main>
    <select id="movies">
      <option value="634649">Spiderman No Way Home</option>
      <option value="1726">Iron Man</option>
      <option value="284053">Thor Ragnarok</option>
      <option value="209112">Batman v Superman</option>
      <option value="299536">Avengers Infinity War</option>
      <option value="91314">Transformers: Age Of Extinction</option>
      <option value="284054">Black Panther</option>
      <option value="550988">Free Guy</option>
      <option value="312221">Creed</option>
      <option value="118340">Guardians Of The Galaxy</option>
    </select>
    <button id="button" @click="getContent()">Get</button>
    <div v-if="content">
      <h1 id="movie-name">
        <u>{{ content.title }}</u>
      </h1>
      <h3 id="overview">Overview: {{ content.overview }}</h3>
      <div id="information">
        <h3 id="release-date">Release date: {{ content.release_date }}</h3>
        <h3 id="popularity">Popularity: {{ content.popularity }}</h3>
        <h3 id="vote-average">Vote average: {{ content.vote_average }}</h3>
        <h3 id="vote-count">Vote count: {{ content.vote_count }}</h3>
        <h3 id="budget">Budget: ${{ content.budget }}</h3>
        <h3 id="original-language">
          Original language: {{ content.original_language }}
        </h3>
        <h3 id="tagline">Tagline: {{ content.tagline }}</h3>
        <h3 id="revenue">Revenue: ${{ content.revenue }}</h3>
        <h3 id="run-time">Run time: {{ content.runtime }} min</h3>
      </div>
      <div id="flex" v-if="trailer">
        <iframe
          id="trailer"
          :src="`https://www.youtube.com/embed/${trailer.at(0).key}`"
          allowfullscreen
        ></iframe>
        <img
          id="poster"
          :src="`https://image.tmdb.org/t/p/original/${content.poster_path}`"
        />
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  background-color: black;
  height: 100vh;
  text-align: center;
  color: rgb(173, 148, 4);
  font-family: math;
}

#title {
  background-color: navy;
  font-size: 5rem;
  text-align: center;
  color: rgb(173, 148, 4);
}

#movie-name {
  margin-bottom: 2rem;
}

main > * {
  margin: 1rem;
}

#movies {
  text-align: center;
  font-size: 20px;
  background-color: white;
}

#button {
  font-size: 20px;
  height: 30px;
  width: 5rem;
  border-radius: 5px;
  background-color: navy;
  color: rgb(173, 148, 4);
  margin: 0;
}

#information {
  display: grid;
  grid-template-columns: auto auto auto;
}

#information > * {
  margin: 1rem;
}

#flex {
  display: flex;
}

#trailer {
  width: 40rem;
  margin-left: 5rem;
}

img {
  height: 30rem;
  margin-left: 5rem;
  border: solid white;
}
</style>
