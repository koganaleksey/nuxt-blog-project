<template>
  <el-form
    ref="form"
    @submit.native.prevent="onSubmit"
    :model="controls"
    :rules="rules"
  >
    <h1>Добавить комментарий</h1>
    <el-form-item label="Ваше имя" prop="name">
      <el-input v-model="controls.name" />
    </el-form-item>
    <el-form-item label="Текст комментария" prop="text">
      <el-input
        v-model="controls.text"
        :rows="2"
        type="textarea"
        resize="none"
      />
    </el-form-item>
    <el-form-item>
      <el-button :loading="loading" type="primary" round native-type="submit"
        >Добавить комментарий</el-button
      >
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  props: {
    postId: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      loading: false,
      controls: {
        name: '',
        text: ''
      },
      rules: {
        name: [
          {
            required: true,
            message: 'Имя не должно быть пустым',
            trigger: 'blur'
          }
        ],
        text: [
          {
            required: true,
            message: 'Введите Ваш комментарий',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(async (valid) => {
        if (valid) {
          this.loading = true
          const formData = {
            name: this.controls.name,
            text: this.controls.text,
            postId: this.postId
          }
          try {
            const newComment = await this.$store.dispatch(
              'comment/create',
              formData
            )
            this.$message.success('Комментарий добавлен')
            this.$emit('created', newComment)
            this.controls.name = ''
            this.controls.text = ''
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

<style lang="scss" scoped></style>
