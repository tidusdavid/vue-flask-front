<template>
  <div id="app">
    <Header />
    <b-container class="bv-example-row">
      <b-row class="mt-5">
        <Index />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Index from "./components/Index.vue";

export default {
  name: "App",
  components: {
    Header,
    Index
  }
};
</script>