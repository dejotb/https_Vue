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
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No stored experiences found. Start adding some results firs.
      </p>
      <ul v-if="!isLoading && results && results.length > 0">
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
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },

  data() {
    return {
      results: [],
      isLoading: false,
    };
  },

  methods: {
    async loadExperiences() {
      this.isLoading = true;
      const data = await fetch(
        'https://vue-http-demo-a17da-default-rtdb.europe-west1.firebasedatabase.app/surveys.json'
      );

      try {
        const response = await data.json();

        const loaded = await false;

        this.isLoading = loaded;

        console.log(response);

        const results = [];

        for (const id in response) {
          results.push({
            id: id,
            name: response[id].name,
            rating: response[id].rating,
          });
        }

        this.results = results;

        console.log(this.results);
      } catch (err) {
        console.log(err);
      }
    },
  },
  mounted() {
    this.loadExperiences();
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
