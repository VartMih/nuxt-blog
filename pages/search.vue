<template>
  <div>
    <Navbar :variable="variable" />
    <SearchHeader @searchPosts="posts = $event"/>
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <nav aria-label="breadcrumb" class="mt-4">
            <ol class="breadcrumb">
              <li class="breadcrumb-item"><nuxt-link to="/">Главная</nuxt-link></li>
              <li class="breadcrumb-item active" aria-current="page">Поиск</li>
            </ol>
          </nav>
          <p class="lead">Найдено записей: {{posts.count}}</p>
          <div v-for="post in posts.results" :key="post.id">
            <nuxt-link :to="`/posts/${post.slug}`"><h2>{{ post.h1 }}</h2></nuxt-link>
            <p v-html="post.description"></p>
            <hr>
          </div>
        </div>
      </div>
    </div>
    <nav class="paginator-outer" aria-label="Paginate me">
      <ul class="pagination">
        <li class="page-item">
          <nuxt-link v-if="previous != null" class="page-link" :to="previous" tabindex="-1"><span aria-hidden="true">«</span></nuxt-link>
          <template v-else class="page-item disabled"><a class="page-link disabled" href="#" tabindex="-1"><span aria-hidden="true">«</span></a></template>
        </li>
        <span v-for="i in total" :key="i">
          <li  v-if="current_page === i || (!$route.query.page && i === 1)" class="page-item active">
            <nuxt-link class="page-link" :to="`?page=${i}&q=${search_query}`">{{i}}</nuxt-link></li>
          <li v-else class="page-item">
            <nuxt-link class="page-link" :to="`?page=${i}&q=${search_query}`">{{i}}</nuxt-link></li>
        </span>
        <li class="page-item">
          <nuxt-link v-if="next != null" class="page-link" :to="next"><span aria-hidden="true">»</span></nuxt-link>
          <template v-else class="page-item disabled">
            <a class="page-link" href="#"><span aria-hidden="true">»</span></a>
          </template>
        </li>
      </ul>
    </nav>
    <br>
  </div>
</template>

<script>
import SearchHeader from "@/components/SearchHeader";
import Navbar from "@/components/Navbar"
import axios from "axios";
export default {
  components: {
    SearchHeader,
    Navbar
    },
  layout: "search",
  watchQuery: ['q', 'page'],
  data() {
    return {
      posts: [],
      total: [],
      next: [],
      previous: [],
      current_page: 0,
      search_query: '',
      variable: 'search'
    }
  },
  async asyncData({route}) {
    let page_and_search = route.query.page !== undefined ? `?page=${route.query.page}&search=${route.query.q}` : `?search=${route.query.q}`;
    const { data } = await axios.get(encodeURI(`http://localhost:8000/api/posts/${page_and_search}`));
    let next = data.next != null ? data.next.split('/')[5] : data.next;
    let previous = data.previous != null ? data.previous.split('/')[5] : data.previous;
    let current_page = route.query.page
    let search_query = route.query.q
    return {
      posts: data,
      total: Math.ceil(data.count / 6),
      next: next,
      previous: previous,
      current_page: Number(current_page),
      search_query: search_query
    }
  },
}
</script>

<style scoped>
</style>