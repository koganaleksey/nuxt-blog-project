<template>
  <el-row>
    <el-col>
      <el-card :body-style="{ padding: '0px' }" shadow="hover" class="post">
        <header slot="header" class="post-header">
          <h3>{{ post.title }}</h3>
          <small class="time"
            ><i class="el-icon-time">&nbsp;</i>{{ post.date | date }}</small
          >
        </header>
        <img :src="post.imageUrl" class="image post-img" alt="Post Image" />
        <footer class="bottom clearfix post-footer">
          <el-button @click="openPost" round>Открыть</el-button>
          <span>
            <i class="el-icon-message"></i>
            {{ post.comments.length }}
          </span>
        </footer>
      </el-card>
    </el-col>
  </el-row>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  methods: {
    openPost() {
      const id = this.post._id
      this.$router.push(`/post/${id}`)
    }
  }
}
</script>

<style lang="scss" scoped>
.post {
  margin-bottom: 2rem;
}
.post-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.post-img {
  width: 100%;
  height: auto;
}
.post-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 18px 20px;
}
</style>
