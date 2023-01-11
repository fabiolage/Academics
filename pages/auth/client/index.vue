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
                    <v-list-item-subtitle>client@gmail.com</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
              <v-divider></v-divider>
              <v-list nav dense>
                <v-list-item link v-on:click="createNewTicket()">
                  <v-list-item-icon>
                    <v-icon>mdi-folder-plus-outline</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>Create ticket</v-list-item-title>
                </v-list-item>
                <v-list-item link v-on:click="displayListOfTickets()">
                  <v-list-item-icon>
                    <v-icon>mdi-list-box-outline</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>My tickets</v-list-item-title>
                </v-list-item>
                <v-divider></v-divider>
                <v-list-item link v-on:click="displayListOfPolicies()">
                  <v-list-item-icon>
                    <v-icon>mdi-shield-car</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>My Policies</v-list-item-title>
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
            <ticket-create v-if="showContainer.showTicketCreate" />
            <ticket-list v-if="showContainer.showListOfTickets" />
            <policies-list v-if="showContainer.showListOfPolicies" />
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
      showContainer: { showTicketCreate: false, showListOfTickets: false, showListOfPolicies: false }
    };
  },
  components: {
    "ticket-create": () => import("@/pages/auth/client/ticket/create.vue"),
    "ticket-list": () => import("@/pages/auth/client/ticket/list.vue"),
    "policies-list": () => import("@/pages/auth/client/policies/list.vue"),
  },
  computed: {
    filteredApproved: function () {
      return this.tickets.filter((ticket) => ticket.ticketStatus === "Approved")
    }
  },
  methods: {
    createNewTicket: function () {
      this.resetDisplayVariable();
      this.showContainer.showTicketCreate = true;
    },
    displayListOfTickets: function () {
      this.resetDisplayVariable();
      this.showContainer.showListOfTickets = true;
    },
    displayListOfPolicies: function () {
      this.resetDisplayVariable();
      this.showContainer.showListOfPolicies = true;
    },
    resetDisplayVariable: function () {
      this.showContainer.showTicketCreate = false;
      this.showContainer.showListOfTickets = false;
      this.showContainer.showListOfPolicies = false;
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
