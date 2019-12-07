<template>
  <div class="page-wrapper">
    <el-breadcrumb :style="{ marginBottom: '1rem' }" separator="/">
      <el-breadcrumb-item to="/admin/list">Посты</el-breadcrumb-item>
      <el-breadcrumb-item>{{ post.title }}</el-breadcrumb-item>
    </el-breadcrumb>

    <el-form
      ref="form"
      @submit.native.prevent="onSubmit"
      :model="controls"
      :rules="rules"
    >
      <el-form-item label="Текст в формате .md или .html" prop="text">
        <el-input
          v-model="controls.text"
          :rows="10"
          type="textarea"
          resize="none"
        />
      </el-form-item>

      <div :style="{ marginBottom: '2rem' }">
        <small :style="{ marginRight: '2rem' }"
          ><i class="el-icon-time"></i>
          <span style="margin-left: 5px">{{ post.date | date }}</span></small
        >
        <small>
          <i class="el-icon-view"></i>
          <span style="margin-left: 5px">{{ post.views }}</span></small
        >
      </div>

      <el-form-item :style="{ marginBottom: '0' }">
        <div class="form-action">
          <el-button
            :loading="loading"
            type="primary"
            round
            native-type="submit"
            >Обновить</el-button
          >
        </div>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  layout: 'admin',
  middleware: ['admin-auth'],
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
      loading: false,
      controls: {
        text: ''
      },
      rules: {
        text: [
          {
            required: true,
            message: 'Текст не должен быть пустым',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  async asyncData({ store, params }) {
    const post = await store.dispatch('post/fetchAdminById', params.id)
    return { post }
  },
  mounted() {
    this.controls.text = this.post.text
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(async (valid) => {
        if (valid) {
          this.loading = true

          const formData = {
            text: this.controls.text,
            id: this.post._id
          }
          try {
            await this.$store.dispatch('post/update', formData)
            this.$message.success('Пост обновлен')
            this.loading = false
          } catch (e) {
            this.loading = false
          }
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.page-wrapper {
  width: 600px;
}
</style>
