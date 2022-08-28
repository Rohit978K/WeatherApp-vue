<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="" class="mt-0">
      <img
        class="logo img-thumbnail"
        style="max-width: 40px"
        src="../assets/logo.png"
      />
      <b-navbar-brand href="#" class="p-2">{{greeting}}</b-navbar-brand>
      <marquee direction="right" class="text-white"
        ><p><i>{{description}}</i></p></marquee
      >
    </b-navbar>
  </div>
</template>
  <script>
import STRINGS from "../utility/string.utility";
export default {
  name: "NavBar",
  data() {
    return {
      greeting: STRINGS.greetingWeather,
      description: STRINGS.description
    };
  },
};
</script>