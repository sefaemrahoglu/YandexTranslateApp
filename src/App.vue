<template>
  <div class="container">
    <h1 class="text-center">Vue-Translator</h1>
    <h5 class="text-center">Vue.js ile Translator</h5>
    <div class="row">
      <div class="col-md-12 text-center">
        <app-TranslateForm
          @translatedEvent="translatedText=$event"
          @historyEvent="history.push($event)"
        ></app-TranslateForm>
        <h3 class="text-success text-center">{{ translatedText }}</h3>
        <app-SearchHistory :history="history"></app-SearchHistory>
      </div>
    </div>
  </div>
</template>

<script>
import TranslateForm from "./components/TranslateForm";
import SearchHistory from "./components/SearchHistory";
export default {
  components: {
    appTranslateForm: TranslateForm,
    appSearchHistory: SearchHistory
  },
  data() {
    return {
      translatedText: "",
      history: []
    };
  }
};
</script>

<style>
body {
  background-color: #dedede;
}
</style>
