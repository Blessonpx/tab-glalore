<script>
import { ref, onMounted } from 'vue'
import config from './config.js'


export default {
  setup() {
    const repos = ref([])
    const selectedForm = ref('form1')


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



    // Form 1 structure
const form1 = ref({
  textbox: '',
  dropdown: '',
  checkbox: false
})

// Form 2 structure
const form2 = ref({
  textbox1: '',
  textbox2: '',
  dropdown1: '',
  dropdown2: '',
  checkbox: false
})

const submitForm = () => {
  if (selectedForm.value === 'form1') {
    console.log('Form 1 data:', form1.value)
    // further process form1.value
  } else {
    console.log('Form 2 data:', form2.value)
    // further process form2.value
  }
}





    return {
      repos,
      selectedForm,
      form1,
      form2,
      submitForm
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

  <div>
  <label for="repo-select">Select a repository:</label>
  <select id="repo-select" v-model="selectedRepo">
    <option disabled value="">-- Select --</option>
    <option v-for="repo in repos" :key="repo.name" :value="repo.name">
      {{ repo.name }}
    </option>
  </select>
  </div>


  <div>
  <!-- Dropdown to select form -->
  <label>Select Form:</label>
  <select v-model="selectedForm">
    <option value="form1">Form 1</option>
    <option value="form2">Form 2</option>
  </select>

  <hr />

  <!-- Render form based on selection -->
  <div v-if="selectedForm === 'form1'">
    <h3>Form 1</h3>
    <input v-model="form1.textbox" placeholder="Text Box 1" />
    <select v-model="form1.dropdown">
      <option disabled value="">Select Option</option>
      <option v-for="repo in repos" :key="repo.name" :value="repo.name">
        {{ repo.name }}
      </option>
      //  <option>A</option>
      // <option>B</option>
    </select>
    <label>
      <input type="checkbox" v-model="form1.checkbox" />Option form 2
    </label>
  </div>

  <div v-else-if="selectedForm === 'form2'">
    <h3>Form 2</h3>
    <input v-model="form2.textbox1" placeholder="Text Box 1" />
    <input v-model="form2.textbox2" placeholder="Text Box 2" />

    <select v-model="form2.dropdown1">
      <option disabled value="">Select Dropdown 1</option>
      <option v-for="repo in repos" :key="repo.name" :value="repo.name">
        {{ repo.name }}
      </option>


      //<option>X</option>
      //<option>Y</option>
    </select>

    <select v-model="form2.dropdown2">
      <option disabled value="">Select Dropdown 2</option>
      <option v-for="repo in repos" :key="repo.name" :value="repo.name">
        {{ repo.name }}
      </option>
      // <option>1</option>
      // <option>2</option>
    </select>

    <label>
      <input type="checkbox" v-model="form2.checkbox" /> Option Form 1
    </label>
  </div>

  <hr />
  <button @click="submitForm">Submit</button>
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

input, select {
  display: block;
  margin: 10px 0;
}
</style>
