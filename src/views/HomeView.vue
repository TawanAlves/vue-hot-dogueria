<template>
  <div class="main-container">
    <Banner />
    <h1>Monte seu Hot-Dog:</h1>
    <HotForm />
  </div>
</template>

<script>
import Banner from "../components/Banner.vue";
import HotForm from "../components/HotForm.vue";
export default {
  name: "Home",
  components: {
    Banner,
    HotForm,
  },
};
</script>
