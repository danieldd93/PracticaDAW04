<template>
  <div class="container">
    <div class="header">
      <img src="https://www.lavanguardia.com/files/image_948_465/uploads/2019/08/26/5fa534dd2a116.png" alt="NASA Logo" class="logo" />
      <h2 class="title">NASA Astronomy Picture of the Day</h2>
      <p class="subtitle">Discover the cosmos!</p>
    </div>

    <q-form @submit.prevent="fetchApod" class="form">
      <q-input v-model="filters.start_date" type="date" label="Start Date" class="input q-mb-sm" />
      <q-input v-model="filters.end_date" type="date" label="End Date" class="input q-mb-sm" />
      <q-btn class="input q-mb-sm" label="Consultar" type="submit" color="primary" />
    </q-form>

    <div v-if="apods.length" class="grid">
      <q-card v-for="(apod, index) in paginatedApods" :key="index" class="card">
        <q-card-section>
          <div class="text-h6">{{ apod.title }}</div>
          <div class="text-subtitle2">{{ apod.date }}</div>
        </q-card-section>
        <q-card-section>
          <img :src="apod.url" :alt="apod.title" v-if="apod.media_type === 'image'" class="card-image" />
          <iframe :src="apod.url" v-if="apod.media_type === 'video'" frameborder="0" class="card-video"></iframe>
          <p class="card-text">{{ apod.explanation }}</p>
        </q-card-section>
      </q-card>
    </div>
    <div v-else>
      <q-spinner-dots color="primary" />
    </div>

    <q-pagination v-model="page" :max="maxPage" class="pagination" />
  </div>
</template>

<script>
import axios from 'axios';
import { QForm, QInput, QBtn, QSpinnerDots, QPagination, QCard, QCardSection } from 'quasar';

export default {
  name: "ApodComponent",
  components: {
    QForm,
    QInput,
    QBtn,
    QSpinnerDots,
    QPagination,
    QCard,
    QCardSection
  },
  data() {
    return {
      apods: [],
      filters: {
        start_date: '',
        end_date: ''
      },
      page: 1,
      perPage: 4
    }
  },
  computed: {
    maxPage() {
      return Math.ceil(this.apods.length / this.perPage);
    },
    paginatedApods() {
      const start = (this.page - 1) * this.perPage;
      const end = start + this.perPage;
      return this.apods.slice(start, end);
    }
  },
  methods: {
    fetchApod() {
      const API_KEY = '5cmLLBIkOFKzPd4TNhbAoA3S4v3XbHnQQ6eDb949';
      const URL = `https://api.nasa.gov/planetary/apod?api_key=${API_KEY}&start_date=${this.filters.start_date}&end_date=${this.filters.end_date}`;

      axios.get(URL)
        .then(response => {
          this.apods = response.data;
          this.page = 1; // Reset to the first page whenever new data is fetched
        })
        .catch(error => {
          console.error("Error fetching APOD data:", error);
        });
    }
  }
}
</script>

<style>
.container {
  max-width: 1200px;
  margin: auto;
  padding: 2em;
  background-color: #f0f0f0;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.header {
  text-align: center;
  margin-bottom: 2em;
}

.logo {
  width: 100px;
  margin-bottom: 1em;
}

.title {
  color: #0b3d91;
  margin-bottom: 0.5em;
}

.subtitle {
  margin-bottom: 2em;
}

.form {
  display: flex;
  gap: 1em;
  justify-content: center;
  margin-bottom: 2em;
}

.input {
  padding: 0.5em;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
}

.button {
  padding: 0.5em 1em;
  background-color: #0b3d91;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
}

.button:hover {
  background-color: #0a3480;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1em;
}

.card {
  background-color: white;
  padding: 1em;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card-title {
  font-size: 1.25em;
  margin-bottom: 0.5em;
}

.card-subtitle {
  color: #666;
  margin-bottom: 1em;
}

.card-image, .card-video {
  width: 100%;
  border-radius: 8px;
  margin-bottom: 1em;
}

.card-text {
  font-size: 1em;
  color: #333;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 2em;
}

.pagination-button {
  padding: 0.5em 1em;
  margin: 0 0.5em;
  background-color: #0b3d91;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
}

.pagination-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>