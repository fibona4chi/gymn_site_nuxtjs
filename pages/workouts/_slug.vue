<template>
<div>
  <Header :title=workout.title />
  <div class="container">
    <div class="row">
      <div class="col-lg-8">
        <nav aria-label="breadcrumb" class="mt-4">
          <ol class="breadcrumb" itemscope itemtype="https://schema.org/BreadcrumbList">
            <li class="breadcrumb-item" itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
            <nuxt-link to="/" itemid="/" itemtype="https://schema.org/Thing" itemscope itemprop="item">
              <span itemprop="name">Главная</span>
            </nuxt-link>
            <meta itemprop="position" content="1">
            </li>
            <li class="breadcrumb-item active" aria-current="page" itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
            <span itemprop="name">{{ workout.title }}</span>
            <meta itemprop="position" content="2">
            </li>
          </ol>
        </nav>
        <img class="img-fluid rounded " :src="workout.image" alt="">
        <hr>
        <span v-html="workout.description"></span>
        <span v-html="workout.content"></span>
        <hr>
        <div class="d-flex">
          <div class="mr-auto p-2 lead">Твоя гимнастика</div>
        </div>
        <hr>
      </div>
      <Aside :tag=tag :aside=aside />
    </div>
  </div>
</div>
</template>

<script>
import axios from "axios";
import post_detail from "@/layouts/w_p_detail";
import Header from "~/components/Header";
import Aside from "@/components/Aside";

export default {
  components: {
    Aside,
    Header
  },
  layout: "w_p_detail",
  async asyncData({params}) {
    const workout = await axios.get(`http://127.0.0.1:8000/api/workouts/${params.slug}`);
    const tag = await axios.get(`http://127.0.0.1:8000/api/tags/`);
    const aside = await axios.get(`http://127.0.0.1:8000/api/aside/`);
    return {
      workout: workout.data,
      tag: tag.data,
      aside: aside.data,
    }
  },
  head() {
    return {
      title: this.workout.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: this.workout.description
        },
        {
          property: "og:url",
          content: `http://localhost:3000${this.$route.path}`
        },
        {
          property: "og:type",
          content: "website"
        },
        {
          property: "og:title",
          content: `${this.workout.title}`
        },
        {
          property: "og:description",
          content: `${this.workout.description}`
        },
        {
          property: "og:site_name", content: "Воздушная гимнастика в Истре"
        },
        {
          property: "og:locale", content: "ru_RU"
        },
        {
          property: "og:image",
          content: "ссылка на картинку"
        },
        {
          property: "og:image:alt",
          content: "Воздушные полотна"
        },
        {
          property: "fb:app_id", content: "23456789"
        },
        {
          name: "twitter:card", content: "summary_large_image"
        },
        {
          name: "twitter:site", content: "http://localhost:3000"
        },
        {
          name: "twitter.title",
          content: `${this.workout.title}`
        },
        {
          name: "twitter:description",
          content: `${this.workout.description}`
        },
        {
          name: "twitter:image:src",
          content: "http://localhost:3000/img"
        }
      ]
    };
  },
}
</script>

<style scoped>

</style>
