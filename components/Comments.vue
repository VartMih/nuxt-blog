<template>
  <div>
    <div class="card my-4">
      <h5 class="card-header aside">Прокомментируй:</h5>
      <client-only>
      <div class="card-body" v-if="loggedIn">
        <form @submit.prevent="addComment">
          <div class="form-group form-comments">
            <textarea class="form-control text-area-comments" rows="3" v-model="new_comment"></textarea>
          </div>
          <button type="submit" class="btn btn-outline-dark" :disabled='!isComplete'>Отправить</button>
        </form>
      </div>
      <div v-else>
        <h6 class="card-header">
          <nuxt-link to="/signin">Авторизуйтесь</nuxt-link> или <nuxt-link to="/signup">зарегистрируйтесь</nuxt-link> чтобы оставить комментарий
        </h6>
      </div>
      </client-only>
    </div>
    <div class="media mb-4 comment" v-for="comment in comments" :key="comment.id">
      <img class="d-flex mr-3 rounded-circle comment-avatar" src="/img/avatar.png" width="50" height="50" alt="">
      <div class="media-body">
        <h5 class="mt-0 ml-2">{{comment.username}}</h5>
        <p class="text-comment">{{comment.text}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'Demo',
  props: ['comments', 'post'],
  data() {
    return {
      new_comment: '',
    }
  },
  methods: {
    async addComment() {
      try {
        let response = await this.$axios.post('http://127.0.0.1:8000/api/comments/', {
        post: this.$props.post.slug,
        username: this.user.username,
        text: this.new_comment,
      })
        this.new_comment = '';
        this.comments.splice(0, 0, response.data)
      console.log(response)
      } catch (err) {
        console.log(err)
      }
    },
  },
  computed: {
    loggedIn() {
      return this.$auth.loggedIn
    },
    user() {
      return this.$auth.user
    },
    isComplete () {
      return this.new_comment;
    }
  }
}
</script>

<style scoped>
</style>