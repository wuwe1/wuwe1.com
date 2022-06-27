<script setup lang='ts'>
const { frontmatter } = defineProps({
  frontmatter: {
    type: Object,
    required: true,
  },
})
</script>

<template>
  <div class="prose">
    <h1>
      {{ frontmatter.title }}
    </h1>
    <article ref="content">
      <slot />
    </article>
  </div>
</template>
