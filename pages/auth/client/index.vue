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
                <v-list-item link v-on:click="createNewTicket()">
                  <v-list-item-icon>
                    <v-icon>mdi-folder-plus-outline</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>Create ticket</v-list-item-title>
                </v-list-item>
                <v-divider></v-divider>
                <v-list-item link v-on:click="displayListOfPolicies()">
                  <v-list-item-icon>
                    <v-icon>mdi-shield-car</v-icon>
                  </v-list-item-icon>
                  <v-list-item-title>My Policies</v-list-item-title>
                </v-list-item>
               <logout></logout>
              </v-list>
            </v-navigation-drawer>
          </v-card>
          <div class="component-container">
            <ticket-create v-if="showContainer.showTicketCreate" />
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
      showContainer: { showTicketCreate: false, showListOfPolicies: false }
    };
  },
  components: {
    "user": () => import("@/pages/menu/user.vue"),
    "logout": () => import("@/pages/menu/logout.vue"),
    "ticket-create": () => import("@/pages/auth/client/occurrences/create.vue"),
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
    displayListOfPolicies: function () {
      this.resetDisplayVariable();
      this.showContainer.showListOfPolicies = true;
    },
    resetDisplayVariable: function () {
      this.showContainer.showTicketCreate = false;
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
