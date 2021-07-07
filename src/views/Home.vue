<template>
  <div class="home">
    <div class="form">
      <br />
      <div class="input-sort">
        <input
          type="text"
          v-model="searchMovies"
          placeholder="Search a movie..."
          class="search-input"
        />
        <div class="dropdown">
          <button class="dropbtn">Filmleri Sırala</button>
          <div class="dropdown-content">
            <a href="#" @click="sortAlphabet">Ada göre (A-Z)</a>
            <a href="#" @click="sortAlphabetReverse">Ada göre (Z-A)</a>

            <a href="#" @click="sortDateDesc">Çıkış Tarihine göre (Artan)</a>
            <a href="#" @click="sortDateAsc">Çıkış Tarihine göre (Azalan)</a>

            <a href="#" @click="sortIdDesc">ID'ye göre (Artan) </a>
            <a href="#" @click="sortIdAsc">ID'ye göre (Azalan)</a>
          </div>
        </div>
      </div>

      <div v-if="searchMovies !== ''" class="autocomplete-popup">
        <span v-for="item in getDataFilter" :key="item.id">
          <router-link :to="{ name: 'Detail', params: { id: item.id } }">
            <div @click="searchMovies = item.title">{{ item.title }}</div>
          </router-link>
        </span>
      </div>
      <br /><br />
      <div class="tags">
        <p v-for="genre in listArr" :key="genre" @click="bring(genre)">
          {{ genre }}
        </p>
      </div>

      <div id="movie-list">
        <div class="movie-card">
          <ul style="list-style-type: none">
            <li v-for="item in randomMovies" :key="item.id">
              <div class="image-container">
                <router-link :to="{ name: 'Detail', params: { id: item.id } }">
                  <img :src="item.poster.toString()" />
                </router-link>

                <figcaption>
                  <p>
                    {{ item.title }}
                  </p>
                </figcaption>
              </div>

              <br />
            </li>
          </ul>
        </div>
      </div>
      <br />
    </div>
  </div>
</template>

<script>
import movies from "./movies.json";

export default {
  data() {
    return {
      movies1: movies,
      searchMovies: "",
      randomMovies: [],
      genres: [],
      myGenres: [],
      totalGenres: [],
      mergedArr: [],
      listArr: [],
      resultArr: [],
      myTarget: [],
    };
  },
  props: {},

  mounted() {
    this.movie = movies.map((item) => item.title);
    let myGenres = movies.map((item) => item.genres);
    this.genres = this.genres.push(myGenres.map((genre) => genre));
    this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
    let myTarget = JSON.parse(JSON.stringify(this.randomMovies));
    this.myTarget = myTarget;
    var totalGenres = [...new Set(myGenres)];

    this.mergedArr = [].concat.apply([], totalGenres);
    this.totalGenres = [...new Set(this.mergedArr)];
    let uniqueArr = JSON.parse(JSON.stringify(this.totalGenres));
    uniqueArr = uniqueArr.filter(function(el) {
      return el != null;
    });
    this.listArr = [...uniqueArr].sort();
  },

  methods: {
    bring(genre) {
      let myArray = JSON.parse(JSON.stringify(this.myTarget));
      let sortedArr = [];
      for (let index = 0; index < myArray.length; index++) {
        if (myArray[index].genres.includes(genre)) {
          sortedArr.push(myArray[index]);
        }
      }
      this.randomMovies = sortedArr;
    },

    sortAlphabet() {
      this.movie = movies.map((item) => item.title);

      this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
      let myTarget = JSON.parse(JSON.stringify(this.randomMovies));

      myTarget.sort(function(a, b) {
        // sort alphabetically
        var nameA = a.title.toUpperCase(); // ignore upper and lowercase
        var nameB = b.title.toUpperCase(); // ignore upper and lowercase
        if (nameA < nameB) {
          return -1;
        }
        if (nameA > nameB) {
          return 1;
        }

        // names must be equal
        return 0;
      });
      this.randomMovies = myTarget;
    },
    sortAlphabetReverse() {
      // sort alphabetically reverse

      this.movie = movies.map((item) => item.title);

      this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
      let myTarget = JSON.parse(JSON.stringify(this.randomMovies));

      myTarget.sort(function(a, b) {
        var nameA = a.title.toUpperCase(); // ignore upper and lowercase
        var nameB = b.title.toUpperCase(); // ignore upper and lowercase
        if (nameA > nameB) {
          return -1;
        }
        if (nameA < nameB) {
          return 1;
        }

        // names must be equal
        return 0;
      });
      this.randomMovies = myTarget;
    },

    sortDateDesc() {
      // sort by date
      this.movie = movies.map((item) => item.title);

      this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
      let myTarget = JSON.parse(JSON.stringify(this.randomMovies));

      myTarget.sort(function(a, b) {
        return a.release_date - b.release_date;
      });
      this.randomMovies = myTarget;
    },

    sortDateAsc() {
      // sort by date
      this.movie = movies.map((item) => item.title);

      this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
      let myTarget = JSON.parse(JSON.stringify(this.randomMovies));

      myTarget.sort(function(a, b) {
        return b.release_date - a.release_date;
      });
      this.randomMovies = myTarget;
    },
    sortIdDesc() {
      // sort by id
      this.movie = movies.map((item) => item.title);

      this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
      let myTarget = JSON.parse(JSON.stringify(this.randomMovies));

      myTarget.sort(function(a, b) {
        return a.id - b.id;
      });
      this.randomMovies = myTarget;
    },

    sortIdAsc() {
      // sort by id
      this.movie = movies.map((item) => item.title);

      this.randomMovies = this.movies1.slice(0, 20).map((item) => item);
      let myTarget = JSON.parse(JSON.stringify(this.randomMovies));

      myTarget.sort(function(a, b) {
        return b.id - a.id;
      });
      this.randomMovies = myTarget;
    },
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
.tags {
  display: flex;
  flex-direction: row;
  p {
    color: #fff;
  }
  p:hover {
    background-color: #3e8e41;
    cursor: pointer;
  }
}
ul {
  width: 100%;
  height: auto;
  display: flex;
  flex-wrap: wrap;

  li {
    width: 20%;
    padding: 10px;
    min-width: 15rem;
  }
}
.image-container {
  width: 100%;
  height: 100%;
  img {
    object-fit: cover;
    object-position: center;
    width: 100%;
    height: auto;
  }
}
/* Dropdown Button */
.dropbtn {
  height: 32px;
  display: flex;
  align-items: center;
  background-color: #04aa6d;
  color: white;
  padding: 16px;
  font-size: 16px;
  border: none;
  flex: 3;
}

/* The container <div> - needed to position the dropdown content */
.dropdown {
  position: relative;
  display: inline-block;
}

/* Dropdown Content (Hidden by Default) */
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f1f1f1;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
}

/* Links inside the dropdown */
.dropdown-content a {
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

/* Change color of dropdown links on hover */
.dropdown-content a:hover {
  background-color: #ddd;
}

/* Show the dropdown menu on hover */
.dropdown:hover .dropdown-content {
  display: block;
}

/* Change the background color of the dropdown button when the dropdown content is shown */
.dropdown:hover .dropbtn {
  background-color: #3e8e41;
}

figcaption {
  text-align: center;
  p {
    font-size: 20px;
    font-weight: 700;
    color: #fff;
  }
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
  span:hover {
    background-color: #04aa6d;
  }
  a {
    color: #f3f3f3;

    &:hover {
      color: #fff;
      font-size: large;
      text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;
    }
  }
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

  .input-sort {
    display: flex;
    align-items: center;
    width: auto;

    input {
      display: flex;
      align-items: center;
      height: 32px;
      outline: none;
      padding: 4px 8px 4px 6px;
      flex: 5;
    }
  }
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
      flex-direction: row;
      flex: wrap;
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
      width: auto;
      height: 32px;
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
}
</style>
