<script setup lang='ts'>
import { useRouter } from 'vue-router'

export interface Post {
  path: string
  title: string
  date: string
}

const router = useRouter()
const routes: Post[] = router.getRoutes()
  .filter(i => i.path.startsWith('/posts') && i.meta.frontmatter.date)
  .sort((a, b) => +new Date(b.meta.frontmatter.date) - +new Date(a.meta.frontmatter.date))
  .map(i => ({
    path: i.path,
    title: i.meta.frontmatter.title,
    date: i.meta.frontmatter.date,
  }))
</script>

<template>
  <div>
    <router-link
      v-for="route in routes"
      :key="route.path"
      :to="route.path"
    >
      <div text-lg text-gray-300:80>
        {{ route.title }}
      </div>
      <div text-sm text-gray-500:80>
        {{ route.date }}
      </div>
    </router-link>
  </div>
</template>
