<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="" class="mt-0">
      <img
        class="logo img-thumbnail"
        style="max-width: 40px"
        src="../assets/logo.png"
      />
      <b-navbar-brand href="#" class="p-2">Hello Weather!</b-navbar-brand>
      <marquee direction="right" class="text-white"
        ><p><i>A simple weather app</i></p></marquee
      >
    </b-navbar>
  </div>
</template>
  <script>
export default {
  name: "NavBar",
  data() {
    return {};
  },
};
</script>