<template>
  <div>
    <transition>
      <div>
        <nuxt-link :to="{ name: 'index' }">Back to home</nuxt-link>
        <div>
          <img
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            alt=""
          />
        </div>
        <div>
          <h1>{{ movie.title }}</h1>
          <p>
            Released:
            {{ movie.release_date }}
          </p>
          <p>Revenue:{{ movie.revenue }}</p>
          <p>{{ movie.overview }}</p>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'MoviePage',
  components: {},
  data() {
    return {
      movie: '',
    }
  },
  transition: 'movie',
  async fetch() {
    await this.getSingleMovie()
  },
  methods: {
    async getSingleMovie() {
      const data = await axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=2359051b7aff545e1146fac446aa7408&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style scoped>
.movie-enter-active,
.movie-leave-active {
  transition: opacity 0.5s;
}
.movie-enter,
.movie-leave-active {
  opacity: 0;
}
</style>
