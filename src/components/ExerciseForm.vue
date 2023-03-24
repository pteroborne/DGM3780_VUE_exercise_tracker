<template>
  <section class="section">
    <div class="container">
      <h1 class="title">Add Exercise</h1>
      <div class="box">
        <form @submit.prevent="submitExercise">

          <div class="field">
            <label class="label">Search Exercise</label>
            <div class="control">
              <input class="input" type="text" v-model="searchTerm" placeholder="Search for exercises"/>
            </div>
          </div>

          <div class="field">
            <label class="label">Filter by Category</label>
            <div class="control">
              <div class="select">
                <select v-model="filterCategory">
                  <option value="all">All</option>
                  <option value="cardio">Cardio</option>
                  <option value="other">Other</option>
                </select>
              </div>
            </div>
          </div>


          <!-- Exercise Type -->
          <div class="field">
            <label class="label">Exercise Type</label>
            <div class="control">
              <div class="select">
                <select v-model="selectedExercise">
                  <option disabled value="">Choose an exercise</option>
                  <option v-for="exercise in filteredExercises" :key="exercise.id" :value="exercise">
                    {{ exercise.name }}
                  </option>
                </select>
              </div>
            </div>
          </div>

          <!-- Date Input -->
          <div class="field">
            <label class="label">Date</label>
            <div class="control">
              <input class="input" type="date" v-model="date" required/>
            </div>
          </div>

          <!-- Exercise Stats -->
          <div v-if="selectedExercise">
            <div class="field" v-if="isCardioCategory(selectedExercise.category)">
              <label class="label">Time (minutes)</label>
              <div class="control">
                <input class="input" type="number" min="1" v-model="time" required/>
              </div>
            </div>
            <div class="field" v-if="isCardioCategory(selectedExercise.category)">
              <label class="label">Distance (meters)</label>
              <div class="control">
                <input class="input" type="number" min="1" v-model="distance" required/>
              </div>
            </div>
            <div class="field" v-if="!isCardioCategory(selectedExercise.category)">
              <label class="label">Sets</label>
              <div class="control">
                <input class="input" type="number" min="1" v-model="sets" required/>
              </div>
            </div>
            <div class="field" v-if="!isCardioCategory(selectedExercise.category)">
              <label class="label">Reps</label>
              <div class="control">
                <input class="input" type="number" min="1" v-model="reps" required/>
              </div>
            </div>
            <div class="field" v-if="!isCardioCategory(selectedExercise.category)">
              <label class="label">Weight (kg)</label>
              <div class="control">
                <input class="input" type="number" min="1" v-model="weight" required/>
              </div>
            </div>
          </div>

          <!-- Submit Button -->
          <div class="field">
            <div class="control">
              <button class="button is-primary">Add Exercise</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
import {store} from './store';

export default {
  data() {
    return {
      selectedExercise: null,
      date: null,
      time: null,
      distance: null,
      sets: null,
      reps: null,
      weight: null,
      searchTerm: '',
      filterCategory: 'all',
    };
  },
  mounted() {
    this.fetchExercises();
  },
  methods: {
    async fetchExercises() {
      try {
        const response = await axios.get('https://wger.de/api/v2/exercise/?language=2&limit=999');
        store.exercises = response.data.results;
      } catch (error) {
        console.error(error);
      }
    },

    isCardioCategory(categoryId) {
      const cardioCategories = [15];
      return cardioCategories.includes(categoryId);
    },

    submitExercise() {
      const exerciseData = {
        exercise: this.selectedExercise,
        date: this.date,
        time: this.time,
        distance: this.distance,
        sets: this.sets,
        reps: this.reps,
        weight: this.weight,
      };

      store.exercisesHistory.push(exerciseData);
      this.resetForm();
    },


    resetForm() {
      this.selectedExercise = null;
      this.date = null;
      this.time = null;
      this.distance = null;
      this.sets = null;
      this.reps = null;
      this.weight = null;
    },
  },
  computed: {
    filteredExercises() {
      let filtered = store.exercises;

      if (this.searchTerm) {
        filtered = filtered.filter((exercise) =>
            exercise.name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }

      if (this.filterCategory !== 'all') {
        filtered = filtered.filter((exercise) =>
            this.filterCategory === 'cardio'
                ? this.isCardioCategory(exercise.category)
                : !this.isCardioCategory(exercise.category)
        );
      }

      return filtered;
    },
  },
};
</script>


