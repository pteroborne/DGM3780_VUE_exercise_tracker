<template>
  <section class="section">
    <div class="container">
      <h1 class="title">Exercise History</h1>
      <table class="table is-fullwidth is-striped">
        <thead>
        <tr>
          <th>Date</th>
          <th>Exercise</th>
          <th v-if="exerciseStats.time">Time (minutes)</th>
          <th v-if="exerciseStats.distance">Distance (meters)</th>
          <th v-if="exerciseStats.sets">Sets</th>
          <th v-if="exerciseStats.reps">Reps</th>
          <th v-if="exerciseStats.weight">Weight (kg)</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="exercise in exercisesHistory" :key="exercise.id">
          <td>{{ exercise.date }}</td>
          <td>{{ exercise.exercise.name }}</td>
          <td v-if="exerciseStats.time">{{ exercise.time || '-' }}</td>
          <td v-if="exerciseStats.distance">{{ exercise.distance || '-' }}</td>
          <td v-if="exerciseStats.sets">{{ exercise.sets || '-' }}</td>
          <td v-if="exerciseStats.reps">{{ exercise.reps || '-' }}</td>
          <td v-if="exerciseStats.weight">{{ exercise.weight || '-' }}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>



<script>
import {store} from "./store";

export default {
  computed: {
    exercisesHistory() {
      return store.exercisesHistory;
    },
    exerciseStats() {
      return this.exercisesHistory.reduce(
          (acc, exercise) => {
            acc.time = acc.time || !!exercise.time;
            acc.distance = acc.distance || !!exercise.distance;
            acc.sets = acc.sets || !!exercise.sets;
            acc.reps = acc.reps || !!exercise.reps;
            acc.weight = acc.weight || !!exercise.weight;
            return acc;
          },
          {time: false, distance: false, sets: false, reps: false, weight: false}
      );
    },
  },
};
</script>


