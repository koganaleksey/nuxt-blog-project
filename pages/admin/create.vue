<template>
  <div class="page-wrapper">
    <el-form
      ref="form"
      @submit.native.prevent="onSubmit"
      :model="controls"
      :rules="rules"
    >
      <h2 :style="{ marginBottom: '1rem' }">Создать новый пост</h2>

      <el-form-item label="Введите название поста" prop="title">
        <el-input v-model="controls.title" />
      </el-form-item>

      <el-form-item
        :style="{ marginBottom: '2rem' }"
        label="Текст в формате .md или .html"
        prop="text"
      >
        <el-input
          v-model="controls.text"
          :rows="10"
          type="textarea"
          resize="none"
        />
      </el-form-item>

      <el-button
        @click="previewDialog = true"
        :style="{ marginBottom: '1rem' }"
        type="mini"
        round
        >Предпросмотр</el-button
      >

      <el-dialog :visible.sync="previewDialog" title="Предпросмотр">
        <!-- <div :key="controls.text"> -->
        <vue-markdown :key="controls.text">{{ controls.text }}</vue-markdown>
        <!-- </div> -->
      </el-dialog>

      <el-upload
        ref="upload"
        :on-change="handleImageChange"
        :auto-upload="false"
        :style="{ marginBottom: '1rem' }"
        drag
        action="https://jsonplaceholder.typicode.com/posts/"
      >
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">
          Перетащите картинку <em>или нажмите</em>
        </div>
        <div slot="tip" class="el-upload__tip">
          файлы с расширениеим jpg/png
        </div>
      </el-upload>

      <el-form-item :style="{ marginBottom: '0' }">
        <div class="form-action">
          <el-button
            :loading="loading"
            type="primary"
            round
            native-type="submit"
            >Создать пост</el-button
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
  head: {
    title: `Создать пост | ${process.env.appName}`
  },
  data() {
    return {
      image: null,
      previewDialog: false,
      loading: false,
      controls: {
        text: '',
        title: ''
      },
      rules: {
        text: [
          {
            required: true,
            message: 'Текст не должен быть пустым',
            trigger: 'blur'
          }
        ],
        title: [
          {
            required: true,
            message: 'Название поста не должно быть пустым',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(async (valid) => {
        if (valid && this.image) {
          this.loading = true

          const formData = {
            title: this.controls.title,
            text: this.controls.text,
            image: this.image
          }
          try {
            await this.$store.dispatch('post/create', formData)
            this.controls.text = ''
            this.controls.title = ''
            this.image = null
            this.$refs.upload.clearFiles()
            this.$message.success('Пост успешно создан')
            this.loading = false
          } catch (e) {
            // eslint-disable-next-line
            console.log(e)
          } finally {
            this.loading = false
          }
        } else {
          this.$message.warning('Форма не валидна')
          this.loading = false
        }
      })
    },
    handleImageChange(file, fileList) {
      this.image = file.raw
    }
  }
}
</script>

<style lang="scss" scoped>
.page-wrapper {
  width: 600px;
}
</style>
