<template>
   <div class="fixed-bottom">
      <v-divider></v-divider>
      <v-list-item class="fixed-bottom m-2" link v-on:click="logout()">
        <v-list-item-icon class="ml-2 mb-2">
          <v-icon>mdi-logout-variant</v-icon>
        </v-list-item-icon>
        <v-list-item-title>Logout</v-list-item-title>
      </v-list-item>
    </div>
</template>
<script>
export default {
  methods: {
    logout: function () {
      this.$router.push(`/`);
    }
  }
}
</script>
