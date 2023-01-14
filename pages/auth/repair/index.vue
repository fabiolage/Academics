<template>
  <div id="app">
    <v-app id="inspire">
      <div class="m-0 p-0 h-100">
        <div class="row m-0">
          <v-card class="h-100">
            <v-navigation-drawer
              permanent
              expand
              class="h-100 position-fixed"
              style="max-height: 100%"
            >
              <user></user>
              <v-list nav dense>
                <v-list-item link v-on:click="reviewOpenTickets()">
                  <v-list-item-icon>
                    <v-icon>mdi-file-find</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>Repairs</v-list-item-title>
                </v-list-item>
                <logout></logout>
              </v-list>
            </v-navigation-drawer>
          </v-card>
          <div class="component-container">
            <list></list>
          </div>
        </div>
      </div>
    </v-app>
  </div>
</template>
<script>
export default {
  data() {
    return {
      showContainer: { showReviewTickets: false },
      occurrences: []
    };
  },
  components: {
    "user": () => import("@/pages/menu/user.vue"),
    "logout": () => import("@/pages/menu/logout.vue"),
    "list": () => import("@/pages/auth/repair/tickets/list.vue")
  },
  computed: {
    filteredTickets() {
      return this.tickets.filter((ticket) => ticket.ticketStatus === "Approved");
    }
  },
  methods: {
    reviewOpenTickets: function () {
      this.resetDisplayVariable();
      this.showContainer.showReviewTickets = true;
    },
    resetDisplayVariable: function () {
      this.showContainer.showReviewTickets = false;
    },
    async fetchOccurrencesForRepair() {
      this.$axios
        .$get("/api/repairCompany/" + this.$auth.user.nif +"/occurrences", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.occurrences = response;
        });
    }
  }
};
</script>
<style scoped>
.component-container {
  width: calc(100% - 275px);
  position: absolute;
  z-index: 0;
  left: 275px;
}
</style>
