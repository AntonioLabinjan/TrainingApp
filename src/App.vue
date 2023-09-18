<template>
  <div>
    <center><h1>Aplikacija za praćenje treninga</h1></center>

    <div>
      <h2>Popis vježbi</h2>
      <ul>
        <li v-for="exercise in exercises" :key="exercise.id">
          {{ exercise.name }} - {{ exercise.duration }} min, {{ exercise.date }}
          Potrošene kalorije: {{ exercise.calories }}
          <button @click="editExercise(exercise)">Uredi</button>
          <button @click="deleteExercise(exercise.id)">Obriši</button>
        </li>
      </ul>
    </div>

    <div>
      <h2>Dodaj novu vježbu</h2>
      <form @submit.prevent="addExercise">
        <label for="name">Naziv:</label>
        <input type="text" id="name" v-model="newExercise.name" required>

        <label for="duration">Trajanje (min):</label>
        <input type="number" id="duration" v-model="newExercise.duration" required>

        <label for="date">Datum:</label>
        <input type="date" id="date" v-model="newExercise.date" required>

        <label for="calories">Potrošene kalorije:</label>
        <input type="number" id="calories" v-model="newExercise.calories" required>

        <button type="submit">Dodaj</button>
      </form>
    </div>

    <div v-if="editingExercise">
      <h2>Uredi vježbu</h2>
      <form @submit.prevent="updateExercise">
        <label for="editName">Naziv:</label>
        <input type="text" id="editName" v-model="editingExercise.name" required>

        <label for="editDuration">Trajanje (min):</label>
        <input type="number" id="editDuration" v-model="editingExercise.duration" required>

        <label for="editDate">Datum:</label>
        <input type="date" id="editDate" v-model="editingExercise.date" required>

        <label for="editCalories">Potrošene kalorije:</label>
        <input type="number" id="editCalories" v-model="editingExercise.calories" required>

        <button type="submit">Spremi promjene</button>
        <button @click="cancelEdit">Otkaži</button>
      </form>
    </div>

    <div>
      <h2>Statistika</h2>
      <p>Ukupno trajanje: {{ totalDuration }} min</p>
      <p>Ukupne potrošene kalorije: {{ totalCalories }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      exercises: [],
      newExercise: {
        name: '',
        duration: 0,
        date: '',
        calories: 0
      },
      editingExercise: null,
      totalDuration: 0,
      totalCalories: 0
    };
  },
  methods: {
    addExercise() {
      this.exercises.push({
        id: Date.now(),
        name: this.newExercise.name,
        duration: this.newExercise.duration,
        date: this.newExercise.date,
        calories: this.newExercise.calories
      });

      this.updateTotals();

      this.newExercise.name = '';
      this.newExercise.duration = 0;
      this.newExercise.date = '';
      this.newExercise.calories = 0;
    },
    editExercise(exercise) {
      this.editingExercise = Object.assign({}, exercise);
    },
    updateExercise() {
      const index = this.exercises.findIndex(ex => ex.id === this.editingExercise.id);

      if (index !== -1) {
        this.exercises[index] = Object.assign({}, this.editingExercise);

        this.updateTotals();

        this.cancelEdit();
      }
    },
    deleteExercise(id) {
      const index = this.exercises.findIndex(ex => ex.id === id);
      if (index !== -1) {
        this.exercises.splice(index, 1);

        this.updateTotals();
      }
    },
    cancelEdit() {
      this.editingExercise = null;
    },
    updateTotals() {
      let totalDuration = 0;
      let totalCalories = 0;

      for (const exercise of this.exercises) {
        totalDuration += parseInt(exercise.duration, 10);
        totalCalories += parseInt(exercise.calories, 10);
      }

      this.totalDuration = totalDuration;
      this.totalCalories = totalCalories;
    }
  }
};
</script>
