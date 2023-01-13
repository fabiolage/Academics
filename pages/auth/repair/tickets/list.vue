<template>
  <v-container class="container">
    <ticket-details
      v-if="viewingDetails"
      :ticketId="selectedTicket"
      @close="viewingDetails = false"
      class="detail"
    />
    <div class="row">
      <div class="col-8">
        <h3>List of Tickets</h3>
      </div>
      <div class="col-4">
        <v-btn class="float-right" @click="toggleFilter">{{ filterOpen ? "Show All" : "Show Open" }}</v-btn>
      </div>
    </div>

    <v-data-table
      :headers="headers"
      :items="filteredTickets"
      :loading="loading"
      class="elevation-1"
    >
      <template v-slot:item.id="{ item }">
        {{ item.id }}
      </template>
      <template v-slot:item.policyNumber="{ item }">
        {{ item.policyNumber }}
      </template>
      <template v-slot:item.productDescription="{ item }">
        {{ item.productDescription }}
      </template>
      <template v-slot:item.covers="{ item }">
        <v-chip v-for="cover in item.covers" :key="cover" class="ma-2">
          {{ cover }}
        </v-chip>
      </template>
      <template v-slot:item.ticketStatus="{ item }">
        {{ item.ticketStatus }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-btn v-if="item.ticketStatus === 'Approved'" @click="viewTicket(item)"
          >Repair</v-btn
        >
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
import TicketDetails from "./details.vue";
export default {
  components: {
    TicketDetails,
  },
  data() {
    return {
      viewingDetails: false,
      selectedTicket: {},
      loading: false,
      filterOpen: true,
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
          ticketStatus: "Approved",
        },
      ],
      headers: [
        { text: "Policy Number", value: "policyNumber" },
        { text: "Product Description", value: "productDescription" },
        { text: "Covers", value: "covers" },
        { text: "Status", value: "ticketStatus" },
        { text: "Actions", value: "actions" },
      ],
    };
  },
  computed: {
    filteredTickets() {
      return this.filterOpen
        ? this.tickets.filter((ticket) => ticket.ticketStatus === "Approved")
        : this.tickets;
    },
  },
  methods: {
    toggleFilter() {
      this.filterOpen = !this.filterOpen;
    },
    viewTicket(item) {
      this.selectedTicket = item.id;
      this.viewingDetails = true;
    },
  },
};
</script>
<style scoped>
.container {
  max-width: none !important;
  width: 100% !important;
}
</style>
