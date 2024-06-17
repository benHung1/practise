<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/effect-cards'
import 'swiper/css/navigation'
import 'swiper/css/pagination'
import { EffectCards, Navigation, Pagination, Autoplay } from 'swiper/modules'

const modules = [EffectCards, Navigation, Pagination, Autoplay]

const articles = ref([])
const currentSlideText = ref('')
const currentSlideUrl = ref('')

const fetchArticles = async () => {
  try {
    const response = await axios.get(
      'https://newsapi.org/v2/everything?domains=wsj.com&apiKey=13efb6857e43407f91da9f5915458736'
    )
    articles.value = response.data.articles.map((article) => ({
      description: article.description,
      url: article.url,
      urlToImage: article.urlToImage
    }))

    if (articles.value.length > 0) {
      currentSlideText.value = articles.value[0].description
      currentSlideUrl.value = articles.value[0].url
    }
  } catch (error) {
    console.error(error)
  }
}

const onSlideChange = (swiper) => {
  currentSlideText.value = articles.value[swiper.activeIndex].description
  currentSlideUrl.value = articles.value[swiper.activeIndex].url
}

onMounted(fetchArticles)
</script>

<template>
  <div class="container">
    <div class="text-section" v-if="articles.length > 0">
      <p>{{ currentSlideText }}</p>
      <a :href="currentSlideUrl" target="_blank" rel="noopener noreferrer">立即閱讀</a>
    </div>
    <swiper
      effect="cards"
      :grabCursor="true"
      :modules="modules"
      navigation
      :pagination="{ clickable: true }"
      class="mySwiper"
      @slideChange="onSlideChange"
      :autoplay="{ delay: 3000, disableOnInteraction: false }"
    >
      <swiper-slide v-for="(article, index) in articles" :key="index">
        <img :src="article.urlToImage" alt="" />
      </swiper-slide>
    </swiper>
  </div>
</template>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 90%;
  margin: 2rem auto;
  gap: 20px;

  @media (max-width: 900px) {
    flex-direction: column;
  }

  .text-section {
    font-size: 1.5rem;
    padding: 20px 20px 20px 0;
    flex: 1;
    display: flex;
    align-items: center;
    flex-direction: column;

    @media (max-width: 900px) {
      order: 2;
    }
    a {
      background: none;
      border: 1px solid #ff9122;
      padding: 1rem;
      border-radius: 3rem;
      color: #ff9122;
      cursor: pointer;
      text-decoration: none;
      margin-top: 2rem;
      font-size: 1rem;
    }
  }

  .mySwiper {
    flex: 1;
    max-width: 55%;
    height: 100%;

    @media (max-width: 900px) {
      max-width: 90%;
    }

    @media (max-width: 450px) {
      overflow: hidden;
    }

    ::v-deep .swiper-button-prev,
    ::v-deep .swiper-button-next {
      color: #fff;
    }

    ::v-deep .swiper-pagination-bullet {
      background: #fff;
    }

    ::v-deep .swiper-pagination-horizontal {
      bottom: 2rem;
    }

    .swiper-slide {
      border-radius: 18px;
      font-size: 22px;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        border-radius: 1rem;
      }
    }
  }
}
</style>
