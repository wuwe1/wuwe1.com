<script setup lang='ts'>
import { useRouter } from 'vue-router'
import { formatDate } from '~/logics'

export interface Post {
  path: string
  title: string
  date: string
  duration: string
}

const router = useRouter()
const routes: Post[] = router.getRoutes()
  .filter(i => i.path.startsWith('/posts') && i.meta.frontmatter.date)
  .sort((a, b) => +new Date(b.meta.frontmatter.date) - +new Date(a.meta.frontmatter.date))
  .map(i => ({
    path: i.path,
    title: i.meta.frontmatter.title,
    date: i.meta.frontmatter.date,
    duration: i.meta.frontmatter.duration,
  }))
</script>

<template>
  <div>
    <router-link v-for="route in routes" :key="route.path" :to="route.path">
      <div text-lg op70>
        {{ route.title }}
      </div>
      <div text-sm op40>
        {{ formatDate(route.date) }}
        <span v-if="route.duration" op80>· {{ route.duration }}</span>
      </div>
    </router-link>
  </div>
</template>
