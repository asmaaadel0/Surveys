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
      <p v-else-if="!isLoading && (results.length === 0 || !results)">
        No stored experience found. start adding some survey results
      </p>
      <ul v-else>
        <survey-data
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-data>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyData from "./SurveyData.vue";

export default {
  // props: ['results'],
  components: {
    SurveyData,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      fetch("https://surveys-6b952-default-rtdb.firebaseio.com/surveys.json")
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = results;
          console.log(data);
        })
        .catch((error) => {
          this.error = "Failed to fetch data - please try again later";
          this.isLoading = false;
          console.log(error);
        });
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
