<template>
  <HeaderCont />
  <main id="main">
    <section class="movie__cont">
      <TitleCont name1="Movie" name2="reference" />
      <div class="container">
        <div class="movie__inner">
          <div class="movie__title">인기있는 스트리밍</div>
          <div class="movie__top">
            <swiper
              :slides-per-view="3"
              :space-between="50"
              :centeredSlides="true"
              :pagination="{
                clickable: true,
              }"
              :modules="modules"
              class="mySwiper"
              @swiper="onSwiper"
              @slideChange="onSlideChange"
            >
              <swiper-slide
                v-for="(topmovie, index) in topmovies"
                :key="topmovie.id"
              >
                <img
                  :src="
                    'https://image.tmdb.org/t/p/w500/' + topmovie.poster_path
                  "
                  :alt="topmovie.title"
                />
                <span class="num">{{ index + 1 }}</span>
                <!-- <span class="title">{{ topmovie.title }}</span> -->
              </swiper-slide>

              <!-- <swiper-slide>Slide 2</swiper-slide>
              <swiper-slide>Slide 3</swiper-slide>
              <swiper-slide>Slide 4</swiper-slide>
              <swiper-slide>Slide 5</swiper-slide>
              <swiper-slide>Slide 6</swiper-slide>
              <swiper-slide>Slide 7</swiper-slide> -->
            </swiper>
          </div>

          <div class="movie__search">
            <form @submit.prevent="SearchMovies()">
              <label for="search" class="sr-only">검색하기</label>
              <input
                type="search"
                id="search"
                placeholder="검색하기"
                v-model="search"
              />
              <button type="submit">검색</button>
            </form>
          </div>
          <div class="movie__list">
            <ul>
              <li v-for="movie in movies" :key="movie.id">
                <img
                  :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
                  :alt="movie.title"
                  @error="replaceByDefault"
                />
                <span class="title">{{ movie.title }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <ContactCont />
    </section>
  </main>
  <FooterCont />
</template>

<script>
import HeaderCont from "@/components/HeaderCont.vue";
import FooterCont from "@/components/FooterCont.vue";
import TitleCont from "@/components/TitleCont.vue";
import ContactCont from "@/components/ContactCont.vue";
import { ref } from "vue";
import img from "@/assets/img/default_img.jpg";

// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from "swiper/vue";

// Import Swiper styles
import "swiper/css";

export default {
  components: {
    HeaderCont,
    FooterCont,
    TitleCont,
    ContactCont,
    Swiper,
    SwiperSlide,
  },

  methods: {
    replaceByDefault(e) {
      e.target.src = img;
    },
  },

  setup() {
    const movies = ref([]);
    const topmovies = ref([]);

    const search = ref("spiderman");

    const onSwiper = (swiper) => {
      console.log(swiper);
    };
    const onSlideChange = () => {
      console.log("slide change");
    };

    const TopMovies = () => {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      fetch(
        "https://api.themoviedb.org/3/movie/popular?api_key=a995ddaefd02120cffba2ce5d93b8eac&language=en-US&page=1",
        requestOptions
      )
        .then((response) => response.json())
        .then((data2) => {
          topmovies.value = data2.results;
          console.log(data2);
        })
        .catch((error) => console.log("error", error));
    };
    TopMovies();

    const SearchMovies = () => {
      var requestOptions = {
        method: "GET",
        redirect: "follow",
      };

      fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=9278d13f704ad0fe53c2263b692efd89&query=${search.value}`,
        requestOptions
      )
        .then((response) => response.json())
        .then((data) => {
          movies.value = data.results;
          console.log(data);
        })
        .catch((error) => console.log("error", error));
    };
    SearchMovies();

    return {
      movies,
      topmovies,
      search,
      SearchMovies,
      onSwiper,
      onSlideChange,
    };
  },
};
</script>

<style lang="scss" scoped>
.movie__title {
  font-size: 3vw;
  margin-bottom: 20px;
  color: #000000;
  font-family: var(--subKor_font);
}
.movie__top {
  margin-bottom: 10%;
  // border: 1px solid #000000;
  position: relative;

  .swiper {
    width: 100%;
    height: 100%;
    position: relative;

    .swiper-slide {
      text-align: center;
      font-size: 18px;
      // background: #fff;
      position: relative;
      z-index: 10;

      /* Center slide text vertically */
      display: -webkit-box;
      display: -ms-flexbox;
      display: -webkit-flex;
      display: flex;
      -webkit-box-pack: center;
      -ms-flex-pack: center;
      -webkit-justify-content: center;
      justify-content: center;
      -webkit-box-align: center;
      -ms-flex-align: center;
      -webkit-align-items: center;
      align-items: center;

      .num {
        position: absolute;
        transform: translate(-50%, -50%);
        top: 132%;
        left: 10%;

        font-family: var(--subKor_font);
        word-spacing: -5px;
        font-size: 14vw;
        line-height: 1;

        color: #dad8d1;
        -webkit-text-stroke: 1px #000000;
        margin-top: -240px;
        z-index: 6;
      }
    }

    .swiper-slide img {
      display: block;
      width: 80%;
      height: 80%;
      object-fit: cover;
    }
  }
}

.movie__search {
  position: relative;

  input {
    border: 2px solid var(--light_border);
    width: 100%;
    background: var(--black);
    border-radius: 50px;
    padding: 1rem 3rem 1rem 2rem;
    color: var(--light_bg);
    font-family: var(--subKor_font);
    margin-bottom: 3%;
  }
  button {
    position: absolute;
    right: 10px;
    top: 9px;
    background: transparent;
    border: 0;
    color: var(--black);
    background: var(--white);
    font-family: var(--subKor_font);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    font-size: 12px;
    transition: opacity 0.3 ease;

    &:active {
      opacity: 0.7;
    }
  }
}
.movie__cont {
  background: var(--dark_bg);
}
.movie__list {
  ul {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 1%;

    li {
      width: 24%;

      .title {
        font-size: 16px;
        font-family: var(—subKor_font);
        display: block;
        padding: 2% 0;
        margin-bottom: 3%;
      }
    }
  }
}
</style>
