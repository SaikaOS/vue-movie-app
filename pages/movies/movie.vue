<template>
  <div>
    <transition>
      <div class="mx-10 my-10">
        <nuxt-link :to="{ name: 'index' }"
          ><p class="my-6 text-md">Back to home</p></nuxt-link
        >
        <div class="flex justify-around items-center">
          <div>
            <img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              alt=""
            />
          </div>
          <div class="w-3/4 flex flex-col justify-between px-6 h-72">
            <h1 class="text-4xl">{{ movie.title }}</h1>
            <p class="text-lg">
              <span>Released:</span>
              {{
                movie.release_date
              }}
            </p>
            <p>Duration: {{movie.runtime}} minutes</p>
            <p class="text-lg">
              <span>Revenue:</span>
              {{
                movie.revenue + '$'
              }}
            </p>
            <p class="text-lg">{{ movie.overview }}</p>
          </div>
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
  transition: 'movie',

  data() {
    return {
      movie: '',
    }
  },
  async fetch() {
    await this.getSingleMovie()
  },
  head() {
    return {
      title: this.movie.title,
    }
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
