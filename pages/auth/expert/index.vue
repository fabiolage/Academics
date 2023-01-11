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
              <v-list>
                <v-list-item class="px-2">
                  <v-list-item-avatar>
                    <v-img
                      src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTucaZx_5ilAhuxxb9dw268yscUKX_ZQivmXqkoS-E&s"
                    ></v-img>
                  </v-list-item-avatar>
                </v-list-item>

                <v-list-item link>
                  <v-list-item-content>
                    <v-list-item-title class="text-h6"> Sandra Adams </v-list-item-title>
                    <v-list-item-subtitle>sandra_a88@gmail.com</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
              <v-divider></v-divider>
              <v-list nav dense>
                <v-list-item link v-on:click="reviewOpenTickets()">
                  <v-list-item-icon>
                    <v-icon>mdi-file-find</v-icon>
                  </v-list-item-icon>
                  <v-badge
                    color="green"
                    :content="filteredTickets.length"
                  >
                  <v-list-item-title>Review tickets</v-list-item-title>
                  </v-badge>
                </v-list-item>
                <div class="fixed-bottom">
                  <v-divider></v-divider>
                  <v-list-item class="fixed-bottom m-2" link>
                    <v-list-item-icon class="ml-2 mb-2">
                      <v-icon>mdi-logout-variant</v-icon>
                    </v-list-item-icon>
                    <v-list-item-title>Logout</v-list-item-title>
                  </v-list-item>
                </div>
              </v-list>
            </v-navigation-drawer>
          </v-card>
          <div class="component-container">
            <ticket-list v-if="showContainer.showReviewTickets" />
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
      tickets: [
        {
          id: 1,
          policyNumber: 12345,
          productDescription: "Home",
          covers: ["Fire", "Theft"],
          ticketStatus: "Open",
        },
        {
          id: 2,
          policyNumber: 67890,
          productDescription: "Auto",
          covers: ["Collision", "Liability"],
          ticketStatus: "Open",
        },
        {
          id: 3,
          policyNumber: 13579,
          productDescription: "Life",
          covers: ["Accidental Death"],
          ticketStatus: "Closed",
        },
      ],
    };
  },
  components: {
    "ticket-list": () => import("@/pages/auth/expert/tickets/list.vue")
  },
  computed: {
    filteredTickets() {
      return this.tickets.filter((ticket) => ticket.ticketStatus === "Open");
    }
  },
  methods: {
    reviewOpenTickets: function () {
      this.resetDisplayVariable();
      this.showContainer.showReviewTickets = true;
    },
    resetDisplayVariable: function () {
      this.showContainer.showReviewTickets = false;
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
