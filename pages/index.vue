<template>
  <div class="min-h-screen">
    <header-page></header-page>
    <div class="mx-6 my-12 flex justify-between items-center">
      <div class="flex items-center justify-between w-2/5">
        <input
          v-model.lazy="searchInput"
          type="text"
          required
          class="rounded-lg p-2 w-96 tracking-wider focus:outline-none focus:border-none"
          placeholder="Search Movies"
          @keyup.enter="$fetch"
        />
        <button :disabled="searchInput === ''" @click="clearSearch">
          Clear
        </button>
      </div>
    </div>
    <div>
      <div
        v-if="searchInput !== ''"
        class="grid grid-cols-5 gap-8 justify-items-center"
      >
        <div
          v-for="(movie, index) in searchedMovies"
          :key="index"
          class="cursor-pointer relative movie"
          @mouseover="hover = index"
          @mouseleave="hover = null"
        >
          <nuxt-link
            class="z-50"
            :to="{ name: 'movies-movie', params: { id: movie.id } }"
          >
            <img
              :src="`https://image.tmdb.org/t/p/w200/${movie.poster_path}`"
              alt=""
              class="rounded-md"
            />
            <div
              v-if="hover === index"
              class="absolute p-2 bottom-0 divs z-10 h-full flex flex-col justify-between"
            >
              <div>
                <p class="text-white font-bold tracking-wider">
                  {{ movie.title }}
                </p>
                <p class="text-white font-bold tracking-wider">
                  {{ movie.release_date.slice(0, 4) }}
                </p>
              </div>
              <p class="text-white text-sm font-bold tracking-wider">
                {{ movie.overview.slice(0, 100) }}...
              </p>
            </div>
          </nuxt-link>
        </div>
      </div>
      <div v-else>
        <upcoming-movies />
        <now-playing-movies />
        <top-rated-movies />
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import HeaderPage from '~/components/Header.vue'
import UpcomingMovies from '~/components/UpcomingMovies'
import NowPlayingMovies from '~/components/NowPlayingMovies'
import TopRatedMovies from '~/components/TopRatedMovies'
export default {
  name: 'IndexPage',
  components: {
    HeaderPage,
    UpcomingMovies,
    NowPlayingMovies,
    TopRatedMovies,
  },
  data() {
    return {
      API: '2359051b7aff545e1146fac446aa7408',
      searchedMovies: [],
      searchInput: '',
      hover: false,
    }
  },
  async fetch() {
    if (this.searchInput !== '') {
      await this.getSearchedMovies()
    }
  },
  methods: {
    async getSearchedMovies() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=${this.API}&language=en-US&page=1&query=${this.searchInput}`
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
    },
    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>
<style>
body {
  background-color: #0f0b0c;
  color: #8a8586;
}
input[type='text'] {
  background-color: #332326;
  font-weight: bold;
  font-size: 16px;
}
button {
  background-color: #332326;
  padding: 5px 8px;
  border-radius: 10px;
}
input::placeholder {
  color: #8a8586;
  font-size: 16px;
}
.movie:hover {
  transform: scale(1.1);
  transition: transform 0.4s ease-in;
}
</style>
