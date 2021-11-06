<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No stored experiences found. Start adding some survey results first.
      </p>
      <ul v-else>
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
      isLoading: false,
      error: null,
    };
  },

  components: {
    SurveyResult,
  },
  mounted() {
    this.loadExperiences();
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      axios
        .get(
          'https://experienceapp-d5b25-default-rtdb.firebaseio.com/surveys.json'
        )
        .then((res) => {
          this.isLoading = false;
          const results = [];
          for (const id in res.data) {
            results.push({
              id: id,
              name: res.data[id].name,
              rating: res.data[id].rating,
            });
          }
          this.results = results;
        })
        .catch(() => {
          this.isLoading = false;
          this.error = 'Failed to fetch data - please try again';
        });
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
