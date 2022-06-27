<script setup lang="ts">

</script>

<template>
  <header flex="" justify-between px-5 py-5>
    <a href="/">
      <img
        w10 h10
        rounded-full
        src="/favicon.jpg"
      >
    </a>
    <nav
      text-xl
      flex="~ gap-4" items-center
    >
      <router-link to="/posts" title="Blog">
        <span>Blog</span>
      </router-link>
      <router-link to="/projects" title="Projects">
        <span>Projects</span>
      </router-link>
      <a
        icon-btn
        rel="noreferrer"
        href="https://twitter.com/wuwe19"
        target="_blank" title="Twitter" i-mdi-twitter
      />
      <a
        icon-btn
        rel="noreferrer"
        href="https://github.com/wuwe1"
        target="_blank" title="Github" i-mdi-github
      />
      <button icon-btn @click="toggleDark()">
        <div dark:i-carbon-moon i-carbon-sun />
      </button>
    </nav>
  </header>
</template>
