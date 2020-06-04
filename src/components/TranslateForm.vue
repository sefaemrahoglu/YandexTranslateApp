<template>
  <form @submit.prevent="translateIt" class="well">
    <textarea
      v-model="translateText"
      cols="30"
      rows="5"
      class="form-control"
      placeholder="Çevirmek istediğiniz kelime/cümle yazınız."
    ></textarea>
    <select v-model="translateTo" class="form-control">
      <option v-for="(value,index) in languages" :value="index" :key="index">{{ value }}</option>
    </select>
    <br />
    <div class="text-left" v-if="detectedLang">
      <strong>Tespit Edilen Dil :</strong>
      {{ detectedLang }}
    </div>
    <br />
    <button type="submit" class="btn btn-primary btn-block">Çevir Gelsin!</button>
  </form>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      translateText: "",
      translateTo: "",
      languages: {},
      detectedLang: ""
    };
  },
  methods: {
    translateIt() {
      let url =
        "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20191217T094730Z.0f486fb41ca7e50a.9c023a39118b7d3683d2bb65d5eaf01e2e42c107&text=" +
        this.translateText +
        "&lang=" +
        this.translateTo;
      axios
        .get(url)
        .then(response => {
          this.detectedLang = this.languages[this.translateTo];
          this.$emit("translatedEvent", response.data.text[0]);

          let history = {
            from: this.languages[response.data.lang.split("-")[0]],
            to: this.detectedLang,
            translateText: this.translateText,
            translatedText: response.data.text[0]
          };

          this.$emit("historyEvent", history)
          this.translateTo = '',
          this.translateText = '',
          this.detectedLang = ''
        })
        .catch(e => console.log(e));
    }
  },
  created() {
    axios
      .get(
        "https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20191217T094730Z.0f486fb41ca7e50a.9c023a39118b7d3683d2bb65d5eaf01e2e42c107&ui=tr"
      )
      .then(response => {
        console.log(response);
        this.languages = response.data.langs;
      })
      .catch(e => console.log(e));
  }
};
</script>

<style>
</style>