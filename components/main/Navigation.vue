<template>
  <nav>
    <nuxt-link
      :style="{ cursor: 'pointer', marginRight: '25rem' }"
      tag="h3"
      to="/"
      >SSR Blog</nuxt-link
    >
    <nuxt-link :style="{ cursor: 'pointer' }" to="/admin" tag="span">
      Admin <i class="el-icon-right"></i>
    </nuxt-link>
  </nav>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
nav {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  border-bottom: 1px solid #e6e6e6;
}
</style>
