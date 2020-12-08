<template>
  <v-app>
    <v-main class="container">
      <UserList />
    </v-main>
  </v-app>
</template>

<script>
import UserList from "./components/UserList";

export default {
  name: 'App',

  components: {
    UserList
  },

  data: () => ({
    //
  }),
};
</script>

<style lang="scss" scoped>
  ::v-deep .container {
    width: 100%;
    max-width: 1200px;
    margin: 40px auto;
  }
</style>
