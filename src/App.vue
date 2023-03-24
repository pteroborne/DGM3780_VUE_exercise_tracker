<template>
  <div id="app">
    <ExerciseForm />
    <ExerciseHistory />
  </div>
</template>

<script>
import ExerciseForm from './components/ExerciseForm.vue';
import ExerciseHistory from './components/ExerciseHistory.vue';

export default {
  components: {
    ExerciseForm,
    ExerciseHistory,
  },
};
</script>
