<template>
  <div class="container">
    <h1 class="title">Lista de Personas de Star Wars</h1>
    <div class="people-container">
      <div v-for="person in people" :key="person.url" class="person-card">
        <div class="person-info">
          <h2 class="person-name">{{ person.name }}</h2>
          <p class="person-data">Año de nacimiento: {{ person.birth_year }}</p>
          <p class="person-data">Género: {{ person.gender }}</p>
          <button @click="showFilms(person)" class="films-button">Ver Películas</button>
        </div>
        <ul v-if="person.films.length > 0" class="films-list">
          <li v-for="film in person.films" :key="film">
            {{ film.title }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      people: [],
    };
  },
  mounted() {
    this.fetchPeople();
  },
  methods: {
    async fetchPeople() {
      try {
        const response = await axios.get('https://swapi.dev/api/people');
        this.people = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    async showFilms(person) {
      try {
        const films = await Promise.all(
          person.films.map((filmUrl) => axios.get(filmUrl))
        );
        person.films = films.map((film) => film.data);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.title {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 2rem;
  color: #FFE81F;
}

.people-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.person-card {
  width: calc(33.33% - 1rem);
  margin-bottom: 2rem;
  border: 1px solid #FFE81F;
  border-radius: 8px;
  padding: 1.5rem;
  background-color: #212121;
  color: #FFF;
}

.person-info {
  margin-bottom: 1.5rem;
}

.person-name {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.person-data {
  margin-bottom: 0.5rem;
}

.films-button {
  background-color: #FFE81F;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-weight: bold;
  color: #212121;
  cursor: pointer;
}

.films-list {
  margin-top: 1rem;
}

@media screen and (max-width: 768px) {
  .person-card {
    width: 100%;
  }
}
</style>
