<template>
  <article class="post">
    <header class="post-header">
      <div class="post-title">
        <h1>{{ post.title }}</h1>
        <nuxt-link to="/">
          <i class="el-icon-back"></i>
        </nuxt-link>
      </div>
      <div class="post-info">
        <small class="time"
          ><i class="el-icon-time">&nbsp;</i>{{ post.date | date }}</small
        >
        <small>
          <i class="el-icon-view">&nbsp;</i>
          {{ post.views }}
        </small>
      </div>
      <div class="post-image">
        <img :src="post.imageUrl" alt="post-image" />
      </div>
    </header>
    <main class="post-content">
      <vue-markdown>{{ post.text }}</vue-markdown>
    </main>
    <footer>
      <app-comment-form
        v-if="canAddComment"
        @created="createCommentHandler"
        :postId="post._id"
      />
      <div v-if="post.comments.length" class="comments">
        <app-comment
          v-for="comment in post.comments"
          :key="comment._id"
          :comment="comment"
        ></app-comment>
      </div>
      <div v-else class="text-center">Комментариев нет</div>
    </footer>
  </article>
</template>

<script>
import AppComment from '~/components/main/Comment'
import AppCommentForm from '~/components/main/CommentForm'

export default {
  components: { AppComment, AppCommentForm },
  head() {
    return {
      title: `${this.post.title} | ${process.env.appName}`
    }
  },
  validate({ params }) {
    return Boolean(params.id)
  },
  data() {
    return {
      canAddComment: true
    }
  },
  async asyncData({ store, params }) {
    const post = await store.dispatch('post/fetchById', params.id)
    await store.dispatch('post/addView', post)
    return {
      post: { ...post, views: ++post.views }
    }
  },
  methods: {
    createCommentHandler(comment) {
      this.post.comments.unshift(comment)
      // this.canAddComment = false
    }
  }
}
</script>

<style lang="scss" scoped>
.post {
  max-width: 600px;
  margin: 0 auto;
}
.post-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}
.post-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.5rem;
}
.post-image {
  img {
    width: 100%;
    height: auto;
  }
}
.post-header {
  margin-bottom: 1.5rem;
}
.post-content {
  margin-bottom: 2rem;
}
</style>
