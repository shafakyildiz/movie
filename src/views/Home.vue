<template>
  <div class="home">
    <div class="form">
      <br />
      <input type="text" v-model="searchMovies" /><br /><br />

      <div v-if="searchMovies !== ''" class="autocomplete-popup">
        <span v-for="item in getDataFilter" :key="item.id">
          <router-link :to="{ name: 'Detail', params: { id: item.id } }">
            <div @click="searchMovies = item.title">{{ item.title }}</div>
          </router-link>
        </span>
      </div>
      <div id="movie-list">
        <ul style="list-style-type: none">
          <li v-for="item in randomMovies" :key="item.id">
            <router-link :to="{ name: 'Detail', params: { id: item.id } }">
              <img :src="item.poster.toString()" />
            </router-link>

            <figcaption>
              <h2>
                {{ item.title }}
              </h2>
            </figcaption>
            <br />
          </li>
        </ul>
      </div>
      <br />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import { ref } from "vue";
// import env from "@/env.js";
import axios from "axios";
import movies from "./movies.json";

export default {
  data() {
    return {
      movies1: movies,
      searchMovies: "",
      randomMovies: [],
    };
  },
  props: {},

  methods: {
    showMovie() {
      axios
        .get(`http://www.omdbapi.com/?apikey=553c1f83&s=${this.searchMovies}`)
        .then((response) => {
          console.log(response.data.Search[0]);
        });
    },
  },

  mounted() {
    this.movie = movies.map((item) => item.title);
    // console.log((this.movie = movies.slice(0, 20).map((item) => item.title)));
    this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
    console.log(this.randomMovies);
  },

  computed: {
    getDataFilter() {
      return this.movies1.filter(
        (item) =>
          item.title
            .toLocaleLowerCase()
            .indexOf(this.searchMovies.toLocaleLowerCase()) > -1
      );
    },
  },
};
</script>

<style lang="scss">
figcaption {
  text-align: center;
}
.autocomplete-popup {
  width: 30%;
  height: 400px;
  overflow-y: auto;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border: 1px solid #eee;

  div {
    padding: 5px 0px 5px 5px;
  }
}
.form {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
  justify-content: center;
}
button {
  display: flex;
  text-align: center;
  justify-content: center;
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
.home {
  .form {
    #movie-list {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #fff;
    }
  }
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }
  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }
  // .movie-list {
  //   display: flex;
  //   flex-wrap: wrap;
  //   margin: 0px 8px;

  //   .movie {
  //     max-width: 50%;
  //     flex: 1 1 50%;
  //     padding: 16px 8px;

  //     .movie-link {
  //       display: flex;
  //       flex-direction: column;
  //       height: 100%;

  //       .product-image {
  //         position: relative;
  //         display: block;

  //         img {
  //           display: block;
  //           width: 100%;
  //           height: 275px;
  //           object-fit: cover;
  //         }

  //         .type {
  //           position: absolute;
  //           padding: 8px 16px;
  //           background-color: #42b883;
  //           color: #fff;
  //           bottom: 16px;
  //           left: 0px;
  //           text-transform: capitalize;
  //         }
  //       }

  //       .detail {
  //         background-color: #496583;
  //         padding: 16px 8px;
  //         flex: 1 1 100%;
  //         border-radius: 0px 0px 8px 8px;

  //         .year {
  //           color: rgb(255, 255, 255);
  //           font-size: 14px;
  //         }

  //         h3 {
  //           color: #fff;
  //           font-weight: 600;
  //           font-size: 18px;
  //         }
  //         a {
  //           color: #fff;
  //         }
  //       }
  //     }
  //   }
  // }
}
</style>