<script setup lang='ts'>
import { formatDate } from '~/logics'
const { frontmatter } = defineProps({
  frontmatter: {
    type: Object,
    required: true,
  },
})
</script>

<template>
  <div class="prose">
    <template v-if="frontmatter.display ?? frontmatter.title">
      <h1>
        {{ frontmatter.display ?? frontmatter.title }}
      </h1>
      <p v-if="frontmatter.date">
        {{ formatDate(frontmatter.date) }}
      </p>
    </template>
    <article ref="content">
      <slot />
    </article>
  </div>
</template>
