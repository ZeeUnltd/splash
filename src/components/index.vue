<template>
  <div>
    <nav class="search-nav">
      <div class="search-bar">
        <div class="flex-div">
          <div class="search-icon input-button">
            <button>
              <img src="../assets/search.svg" alt="Magnifier" />
            </button>
          </div>
          <div class="search-input">
            <input v-model="search" placeholder="Search" />
          </div>
          <div class="input-button">
            <button>
              <svg
                class="BAGAv _1azRR _18QGm"
                version="1.1"
                viewBox="0 0 32 32"
                width="16"
                height="16"
                aria-hidden="false"
                style="fill: #93969a"
              >
                <path
                  d="M25.33 8.55l-1.88-1.88-7.45 7.45-7.45-7.45-1.88 1.88 7.45 7.45-7.45 7.45 1.88 1.88 7.45-7.45 7.45 7.45 1.88-1.88-7.45-7.45z"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </nav>
    <div v-if="loading">
      <img src="../assets/copper-loader.gif" alt="Loader" class="loader-gif" />
      <p>
        <em v-if="!errorMsg">Loading...</em>
        <span v-else>{{errorMsg}}</span>
      </p>
    </div>
    <div class="picture-zone grid" v-else>
      <figure v-for="(image, index) in images" :key="`${index+'-'+image.id}`" class="card">
        <img :src="image.urls.small" :alt="image.alt_description" />
        <figcaption class="image-description">
          <h3 class="bold">{{ image.user.name}}</h3>
          <p>{{ image.user.location}}</p>
        </figcaption>
      </figure>
    </div>
  </div>
</template>

<script>
import("../assets/main.css");

export default {
  name: "SearchPage",
  data() {
    return {
      errorMsg: "",
      images: [],
      loading: false,
      search: ""
    };
  },
  methods: {
    getPhotos() {
      this.loading = true;
      // const Unsplash = require("unsplash-js").default;

      // const unsplash = new Unsplash({
      //   applicationId: process.env.VUE_APP_ACCESS_KEY,
      //   secret: process.env.VUE_APP_SECRET_KEY
      // });
      // console.log(this.myKey);
      let url =
        "https://api.unsplash.com/search/photos?" +
        "client_id=" +
        process.env.VUE_APP_ACCESS_KEY +
        "&per_page=10&page=1&query=african";
      window
        .fetch(url)
        .then(res => res.json())
        .then(res => {
          this.images = res.results;
          this.loading = false;
        })
        .catch(res => {
          console.log(res);
          this.loading = true;
          this.errorMsg = "Oops! Something went wrong";
        });
    }
  },
  computed: {},
  mounted() {
    this.getPhotos();
  }
};
</script>
