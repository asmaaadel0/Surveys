<template>
  <survey-form @survey-submit="storeSurvey"></survey-form>
  <survey-results :results="savedSurveyResults"></survey-results>
</template>

<script>
import SurveyForm from './components/survey/SurveyForm.vue';
import SurveyResults from './components/survey/SurveyResults.vue';



export default {
  components: {
    SurveyForm,
    SurveyResults,
  },
  data() {
    return {
      savedSurveyResults: [],
    };
  },
  methods: {
    storeSurvey(surveyData) {
      const surveyResult = {
        name: surveyData.userName,
        rating: surveyData.rating,
        id: new Date().toISOString(),
      };
      this.savedSurveyResults.push(surveyResult);
      console.log(surveyResult);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>