<template>
  <div>
    <div class="slider middle">
      <div class="slides">
        <input id="r1" type="radio"  name="r" checked />
        <input id="r2" type="radio"  name="r" />
        <input id="r3"  type="radio" name="r" />
        <input id="r4" type="radio"  name="r" />
        <input id="r5" type="radio"  name="r" />

        <div
          v-for="(movie, index) in popularMovies.slice(0, 5)"
          :key="index"
          class="slide"
          :class="{ s1: index === 0 }"
        >
          <nuxt-link
            class="z-50"
            :to="{ name: 'movies-movie', params: { id: movie.id } }"
          >
            <img
              :src="`https://image.tmdb.org/t/p/original/${movie.backdrop_path}`"
              alt=""
              class="object-cover"
            />
            <div
              class="absolute z-10 top-24 left-12 text-white flex flex-col justify-between h-1/2"
            >
              <h1 class="text-6xl tracking-wider">
                {{ movie.original_title }}
              </h1>
              <p class="w-10 p-2 text-center tracking-wider rounded-full rate">
                {{ movie.vote_average }}
              </p>
              <p class="w-1/2 tracking-wider">
                {{ movie.overview }}
              </p>
            </div>
          </nuxt-link>
        </div>
      </div>
      <div class="navigation">
        <label for="r1" class="bar"></label>
        <label for="r2" class="bar"></label>
        <label for="r3" class="bar"></label>
        <label for="r4" class="bar"></label>
        <label for="r5" class="bar"></label>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'HeaderPage',
  data() {
    return {
      popularMovies: [],
    }
  },
  mounted() {
    this.fetch()
  },
  methods: {
    async fetch() {
      await this.getPopularMovies()
    },
    async getPopularMovies() {
      const data = await axios.get(
        'https://api.themoviedb.org/3/movie/popular?api_key=2359051b7aff545e1146fac446aa7408&language=en-US&page=1'
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.popularMovies.push(movie)
      })
    },
  },
}
</script>
<style scoped>
.slider {
  width: 100%;
  height: 600px;
  overflow: hidden;
}
.navigation {
  position: absolute;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
}
.bar {
  height: 8px;
  width: 30px;
  margin: 6px;
  cursor: pointer;
  background-color: #ddd;
  opacity: 0.7;
  border-radius: 10px;
  transition: all 0.4s ease;
}
.bar:hover {
  opacity: 1;
  transform: scale(1.1);
}
input[name='r'] {
  position: absolute;
  visibility: hidden;
}

.slides {
  width: 500%;
  height: 100%;
  display: flex;
}
.slide {
  width: 20%;
  transition: all 0.6s ease;
  position: relative;
}
.slide img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.rate {
  background-color: #332326;
  color: white;
}

#r1:checked ~ .s1 {
  margin-left: 0;
}

#r2:checked ~ .s1 {
  margin-left: -20%;
}

#r3:checked ~ .s1 {
  margin-left: -40%;
}

#r4:checked ~ .s1 {
  margin-left: -60%;
}

#r5:checked ~ .s1 {
  margin-left: -80%;
}
</style>
