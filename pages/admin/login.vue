<template>
  <el-card :style="{ width: '500px' }" shadow="always">
    <div slot="header">
      <h2>Войти в панель администратора</h2>
    </div>
    <el-form
      ref="form"
      @submit.native.prevent="onSubmit"
      :model="controls"
      :rules="rules"
    >
      <el-form-item label="Логин" prop="login">
        <el-input v-model.trim="controls.login" />
      </el-form-item>
      <el-form-item
        :style="{ marginBottom: '2rem' }"
        label="Пароль"
        prop="password"
      >
        <el-input v-model.trim="controls.password" type="password" />
      </el-form-item>
      <el-form-item :style="{ marginBottom: '0' }">
        <div class="form-action">
          <el-button
            :loading="loading"
            type="primary"
            round
            native-type="submit"
            >Войти</el-button
          >
          <nuxt-link :style="{ cursor: 'pointer' }" to="/" tag="span">
            <i class="el-icon-back"></i>&nbsp;На главную
          </nuxt-link>
        </div>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
export default {
  layout: 'empty',
  head: {
    title: `Вход | ${process.env.appName}`
  },
  data() {
    return {
      loading: false,
      controls: {
        login: '',
        password: ''
      },
      rules: {
        login: [
          {
            required: true,
            message: 'Введите логин',
            trigger: 'blur'
          }
        ],
        password: [
          {
            required: true,
            message: 'Введите пароль',
            trigger: 'blur'
          },
          {
            min: 6,
            message: 'Пароль должен быть не менее 6 символов',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  mounted() {
    const { message } = this.$route.query

    switch (message) {
      case 'login':
        this.$message.info('Авторизуйтесь для входа в систему')
        break
      case 'logout':
        this.$message.success('Вы успешно вышли из системы')
        break
      case 'session':
        this.$message.warning(
          'Время сессии истекло, пожалуйста авторизируйтесь заново'
        )
        break
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(async (valid) => {
        if (valid) {
          this.loading = true
          try {
            const formData = {
              login: this.controls.login,
              password: this.controls.password
            }

            await this.$store.dispatch('auth/login', formData)
            this.$router.push('/admin')
            this.$message.success('Вы успешно вошли в систему')
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
.form-action {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
