<script setup lang='ts'>
defineProps<{ projects: Record<string, any[]> }>()
</script>

<template>
  <template v-for="key in Object.keys(projects)" :key="key">
    <h4 text-lg font-bold mb-4>
      {{ key }}
    </h4>
    <div class="project-grid">
      <div v-for="item in projects[key]" :key="item.link" p4>
        <a :href="item.link">
          <div flex items-center>
            <div v-if="item.icon" pt-1 pr-4>
              <div text-4xl op30 :class="item.icon" />
            </div>
            <div flex="~ col">
              <div op60 text-xl>
                {{ item.name }}
              </div>
              <div op40 text-sm>
                {{ item.desc }}
              </div>
            </div>
          </div>
        </a>
      </div>
    </div>
  </template>
</template>

<style scoped>
.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.project-grid>div:hover {
  background: #88888808;
}
</style>
