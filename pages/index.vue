<template>

  <div class="container">

    <h1 class="my-3">Вывод наименования тренировки</h1>
    <div class="row">
      <div v-for="workout in workouts" :key="workout.slug" class="col-md-4">
          <div class="card mb-4 shadow-sm">
            <img :src="workout.image" alt="" class="card-img-top">
            <div class="card-body">
              <h4 class="card-title">{{ workout.title }}</h4>
              <div v-html="workout.description" class="truncate"></div>
              <div class="d-flex justify-content-between align-items-center">
                <div class="btn-group">
                  <nuxt-link :to="`/workouts/${workout.slug}`" class="btn btn-sm btn-outline-secondary">Подробнее</nuxt-link>
                </div>
              </div>
            </div>
          </div>
      </div>
    </div>

    <div>
    <h1 class="my-3">Последние записи блога</h1>
    <div class="row">
      <div v-for="post in posts" :key="post.slug" class="col-md-4">
          <div class="card mb-4 shadow-sm">
            <img :src="post.image" alt="" class="card-img-top">
            <div class="card-body">
              <h4 class="card-title">{{ post.title }}</h4>
              <div v-html="post.description" class="truncate"></div>
              <div class="mb-2">
                <span v-for="tg in post.tag">
                  <nuxt-link :to="`/tags/${tg}`" class="mr-1 badge badge-info">#{{ tg }}</nuxt-link>
                </span>
              </div>
              <div class="d-flex justify-content-between align-items-center">
                <div class="btn-group">
                  <nuxt-link :to="`/posts/${post.slug}`" class="btn btn-sm btn-outline-secondary">Подробнее</nuxt-link>
                </div>
                <small class="text-muted">{{ post.created_at }}</small>
              </div>
            </div>
          </div>
      </div>
    </div>

    <nav aria-label="Paginate me">
      <ul class="pagination justify-content-center">
        <nuxt-link v-if="previous_p != null" class="page-link" :to="previous_p" tabindex="-1">Предыдущая</nuxt-link>
        <li v-else class="page-item disabled">
          <a class="page-link disabled" href="#" tabindex="-1">Предыдущая</a>
        </li>
        <span v-for="i in total_p">
          <li  v-if="current_page_p === i || ($route.query.page === '/' && i === 1)" class="page-item active">
            <nuxt-link class="page-link" :to="`?page=${i}`">{{i}}</nuxt-link></li>
          <li v-else class="page-item">
            <nuxt-link class="page-link" :to="`?page=${i}`">{{i}}</nuxt-link></li>
        </span>
        <nuxt-link v-if="next_p != null" class="page-link" :to="next_p">Следующая</nuxt-link>
        <li v-else class="page-item disabled">
          <a class="page-link" href="#">Следующая</a>
        </li>
      </ul>
    </nav>
    <br>
    </div>

  </div>


</template>

<script>
import axios from "axios";
export default {
  // достаю из урла page
  watchQuery: ['page'],
  // переменные для хранения данных, полученных из апихи
  data() {
    return {
      posts: [],
      total_p: [],
      next_p: [],
      previous_p: [],
      current_page_p: 0
    }
  },
  async asyncData({route}) {
    // console.log(route.query) вернёт, к примеру, {page: "2"} - 2
    console.log(route.query.page);

    // page_p подставляю в урл GET запроса(page беру из route)
    let page_p = route.query.page !== undefined ? `?page=${route.query.page}` : '';
    console.log(page_p);

    const workouts = await axios.get(`http://127.0.0.1:8000/api/workouts/`);

    // GET запрос к апихе
    const posts = await axios.get(`http://127.0.0.1:8000/api/posts/${page_p}`);
    // адрес следующей/предыдущей страницы
    let next_p = posts.data.next != null ? posts.data.next.split('/')[5] : posts.data.next;
    let previous_p = posts.data.previous != null ? posts.data.previous.split('/')[5] : posts.data.previous;

    let current_page_p = route.query.page
    return {
      // возврат данных в Nuxt
      workouts: workouts.data.results,
      posts: posts.data.results,

      total_p: Math.ceil(posts.data.count / 3),
      next_p: next_p,
      previous_p: previous_p,
      current_page_p: Number(current_page_p),
    }
  },
  head() {
    return {
      title: "Гимнастика Истра",
      meta: [
        { hid: "description", name: "description",
        content: "Групповые и индивидуальные занятия по воздушной гимнастике в Истре. Растяжка для детей и взрослых. Подготовка к цирковому училищу. Фитнес, спортивные тренировки"},
        { hid: "keywords", name: "keywords",
        content: "гимнастика, Истра, воздушные полотна, растяжка, воздушное кольцо, фитнес Истра, дети Истра, спорт Истра, Занятия для детей в Истре, секции Истра, воздушная гимнастика"},
      ]
    }
  },
}
</script>

<style>

</style>
