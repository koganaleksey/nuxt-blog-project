<template>
  <div>
    <h1>Аналитика по постам</h1>
    <app-analytics-chart
      :labels="views.labels"
      :data="views.data"
      title="Количество просмотров"
    />
    <app-analytics-chart
      :labels="comments.labels"
      :data="comments.data"
      title="Количество комментариев"
    />
  </div>
</template>

<script>
import AppAnalyticsChart from '~/components/admin/AnalyticsChart'

export default {
  layout: 'admin',
  middleware: ['admin-auth'],
  head: {
    title: `Аналитика | ${process.env.appName}`
  },
  components: {
    AppAnalyticsChart
  },
  async asyncData({ store }) {
    const { views, comments } = await store.dispatch('post/getAnalytics')
    return { views, comments }
  }
}
</script>
