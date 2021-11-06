<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <ul>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import axios from 'axios';
import SurveyResult from './SurveyResult.vue';

export default {
  data() {
    return {
      results: [],
    };
  },

  components: {
    SurveyResult,
  },
  methods: {
    loadExperiences() {
      axios
        .get(
          'https://experienceapp-d5b25-default-rtdb.firebaseio.com/surveys.json'
        )
        .then((res) => {
          const results = [];
          for (const id in res.data) {
            results.push({
              id: id,
              name: res.data[id].name,
              rating: res.data[id].rating,
            });
          }
          this.results = results;
        });
    },
    showSurveys() {
      console.log(this.surveys);
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
