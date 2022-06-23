<template>
  <div class="flex flex-col justify-between mx-4 my-12">
    <h1 class="mb-6 ml-2 text-4xl">Now Playing Movies</h1>
    <div class="grid grid-cols-5 gap-6 justify-items-center">
      <div
        v-for="(movie, index) in nowPlayingMovies.slice(0, 10)"
        :key="index"
        class="cursor-pointer relative movie"
        @mouseover="hover = index"
        @mouseleave="hover = null"
      >
        <nuxt-link class="z-50" :to="{ name: 'movies-movie', params: { id: movie.id } }">
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
            <p class="text-white font-bold tracking-wider">{{ movie.title }}</p>
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
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'NowPlayingMovies',
  data() {
    return {
      nowPlayingMovies: [],
      hover: false,
    }
  },
  async fetch() {
      await this.getNowPlayingMovies()
    },
  methods: {
    async getNowPlayingMovies() {
      const data = await axios.get(
        'https://api.themoviedb.org/3/movie/now_playing?api_key=2359051b7aff545e1146fac446aa7408&language=en-US&page=1'
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.nowPlayingMovies.push(movie)
      })
    },
  },
}
</script>
<style scoped>
.rate {
    background-color: #332326;
    color: white;
}
.movie:hover {
  transform: scale(1.1);
  transition: transform 0.4s ease-in;
}
</style>
