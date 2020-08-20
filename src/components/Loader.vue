<template>
  <div>
    <blockquote>
      <p>{{ quote.quote }}</p>
      <footer>{{ quote.author }}</footer>
    </blockquote>
    <div>
      <b-loading :is-full-page="false" :active.sync="this.isLoading">
      </b-loading>
    </div>
  </div>
</template>

<script>
import quotes from "../assets/quotes";
export default {
  name: "Loader",
  props: {
    isLoading: Boolean
  },
  data: function() {
    return {
      quote: {}
    };
  },
  mounted: function() {
    const index = Math.floor(Math.random() * quotes.length);
    this.quote = quotes[index];
  }
};
</script>
