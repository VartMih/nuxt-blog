<template>
  <div class="container">
    <h1 class="my-3">Последние записи блога</h1>
    <div class="row">
      <div v-for="post in posts" :key="post.slug" class="col-md-4">
          <div class="card mb-4 shadow-sm">
            <img :src="post.image" alt="" class="card-img-top">
            <div class="card-body">
              <h4 class="card-title">{{ post.h1 }}</h4>
              <div v-html="post.description" class="truncate"></div>
              <div class="mb-2">
              <span v-for="tag in post.tags" :key="tag">
                <nuxt-link :to="`/tags/${tag}`" class="mr-1 badge badge-info">#{{ tag }}</nuxt-link>
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
    <nav class="paginator-outer" aria-label="Paginate me">
      <ul class="pagination">
        <li class="page-item">
          <nuxt-link v-if="previous != null" class="page-link" :to="previous" tabindex="-1"><span aria-hidden="true">«</span></nuxt-link>
          <div v-else class="page-item disabled">
            <a class="page-link disabled" href="#" tabindex="-1"><span aria-hidden="true">«</span></a></div>
        </li>
        <span v-for="i in total" :key="i">
          <li  v-if="current_page === i || (!$route.query.page && i === 1)" class="page-item active">
            <nuxt-link class="page-link" :to="`?page=${i}`">{{i}}</nuxt-link></li>
          <li v-else class="page-item">
            <nuxt-link class="page-link" :to="`?page=${i}`">{{i}}</nuxt-link></li>
        </span>
        <li class="page-item">
          <nuxt-link v-if="next != null" class="page-link" :to="next"><span aria-hidden="true">»</span></nuxt-link>
          <template v-else class="page-item disabled">
            <a class="page-link" href="#"><span aria-hidden="true">»</span></a></template>
        </li>
      </ul>
    </nav>
    <br>
  </div>
</template>

<script>
import axios from "axios";
export default {
  watchQuery: ['page'],
  data() {
    return {
      posts: [],
      total: [],
      next: [],
      previous: [],
      current_page: 0
    }
  },
  async asyncData({route}) {
    let page = route.query.page !== undefined ? `?page=${route.query.page}` : '';
    const { data } = await axios.get(`http://127.0.0.1:8000/api/posts/${page}`);
    let next = data.next != null ? data.next.split('/')[5] : data.next;
    let previous = data.previous != null ? data.previous.split('/')[5] : data.previous;
    let current_page = route.query.page
    return {
      posts: data.results,
      total: Math.ceil(data.count / 6),
      next: next,
      previous: previous,
      current_page: Number(current_page)
    }
  },
}
</script>

<style>

</style>