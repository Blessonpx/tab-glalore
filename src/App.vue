<script>
import { ref, onMounted } from 'vue'
import config from './config.js'


export default {
  setup() {
    const repos = ref([])

    const fetchRepos = async () => {
      try {
        const response = await fetch(`${config.apiBaseUrl}/eg/getRepos`)
        const data = await response.json()

        // Filter only repos that match names in config.defaultRepos
        const filtered = data.filter(repo =>
          config.defaultRepos.includes(repo.name)
        )
        repos.value = filtered
        // repos.value = data
        console.log('Fetched Repos:', data)
      } catch (error) {
        console.error('Failed to fetch repos:', error)
      }
    }

    onMounted(() => {
      fetchRepos()
    })

    return {
      repos
    }
  }
}
</script>

<template>
  <div>
    <a href="https://vite.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>

  <div>
    <h2>Git Repositories</h2>

    <ul v-if="repos.length > 0">
      <li v-for="repo in repos" :key="repo.id">
        <strong>{{ repo.name }}</strong><br />
      </li>
    </ul>

    <p v-else>Loading repositories...</p>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
