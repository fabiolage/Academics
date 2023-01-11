<template>
  <div class="mr-4">
    <ticket-details v-if="viewingDetails" :ticket="selectedTicket" @close="viewingDetails = false" />
    <ticket-service-repair v-if="viewingSetServiceDetails" :ticket="selectedTicket" @close="viewingSetServiceDetails = false" />
    <v-select
      v-model="filter"
      :items="statusOptions"
      label="Filter by status"
    ></v-select>
    <v-data-table
      :headers="headers"
      :items="filteredTickets"
      class="elevation-1"
      v-if="filteredTickets.length > 0"
    >
      <template v-slot:item.policyNumber="{ item }">
        {{ item.policyNumber }}
      </template>
      <template v-slot:item.productDescription="{ item }">
        {{ item.productDescription }}
      </template>
      <template v-slot:item.ticketStatus="{ item }">
        {{ item.ticketStatus }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-btn color="primary" @click="viewDetails(item)">View Details</v-btn>
        <v-btn v-if="item.ticketStatus =='Open'" color="error" @click="cancelTicket(item)">Cancel Ticket</v-btn>
        <v-btn v-if="item.ticketStatus =='Approved'" color="normal" @click="viewServiceRepair(item)">Set service repair</v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import TicketDetails from './details.vue'
import TicketServiceRepair from './serviceRepair.vue'
export default {
  components: {
    TicketDetails,
    TicketServiceRepair
  },
  data() {
    return {
      viewingDetails: false,
      viewingSetServiceDetails: false,
      selectedTicket: {},
      filter: '',
      statusOptions: ['Open', 'Closed', 'Canceled', 'Denyed', 'Approved'],
      headers: [
        { text: 'Policy Number', value: 'policyNumber' },
        { text: 'Product Description', value: 'productDescription' },
        { text: 'Ticket Status', value: 'ticketStatus' },
        { text: 'Actions', value: 'actions', sortable: false }
      ],
      tickets: [
        {
          id: 123,
          policyNumber: '12345',
          productDescription: 'Car Insurance',
          ticketStatus: 'Open'
        },
        {
          id: 456,
          policyNumber: '67890',
          productDescription: 'Home Insurance',
          ticketStatus: 'Closed'
        },
        {
          id: 789,
          policyNumber: 'ABC123',
          productDescription: 'Pet Insurance',
          ticketStatus: 'Canceled'
        },
        {
          id: 1,
          policyNumber: 'ABC456',
          productDescription: 'Home Insurance',
          ticketStatus: 'Denyed'
        },
        {
          id: 2,
          policyNumber: 'ABC789',
          productDescription: 'Health Insurance',
          ticketStatus: 'Approved'
        }
      ]
    }
  },
  computed: {
    filteredTickets() {
      if (this.filter) {
        return this.tickets.filter(ticket => ticket.ticketStatus === this.filter)
      }
      return this.tickets
    }
  },
  methods: {
    cancelTicket(item) {
      console.log(`Canceling ticket with policy number: ${item.policyNumber}`)
    },
    viewDetails(item) {
      this.selectedTicket = item.id
      this.viewingDetails = true
      this.viewingSetServiceDetails = false
    },
    viewServiceRepair(item) {
      this.selectedTicket = item.id
      this.viewingDetails = false
      this.viewingSetServiceDetails = true
    }
  }
}
</script>
