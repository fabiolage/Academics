<template>
  <div class="mr-4">
    <occurence-details v-if="viewingDetails" :occurence="selectedOccurence" @close="viewingDetails = false" />
    <occurence-service-repair v-if="viewingSetServiceDetails" :occurence="selectedOccurence" @close="viewingSetServiceDetails = false" />
    <v-select
      v-model="filter"
      :items="statusOptions"
      label="Filter by status"
    ></v-select>
    <v-data-table
      :headers="headers"
      :items="filteredOccurences"
      class="elevation-1"
      v-if="filteredOccurences.length > 0"
    >
      <template v-slot:item.policyNumber="{ item }">
        {{ item.policyNumber }}
      </template>
      <template v-slot:item.productDescription="{ item }">
        {{ item.productDescription }}
      </template>
      <template v-slot:item.occurenceStatus="{ item }">
        {{ item.occurenceStatus }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-btn color="primary" @click="viewDetails(item)">View Details</v-btn>
        <v-btn v-if="item.occurenceStatus =='Open'" color="error" @click="cancelOccurence(item)">Cancel Occurence</v-btn>
        <v-btn v-if="item.occurenceStatus =='Approved'" color="normal" @click="viewServiceRepair(item)">Set service repair</v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import OccurenceDetails from './details.vue'
import OccurenceServiceRepair from './serviceRepair.vue'
export default {
  components: {
    OccurenceDetails,
    OccurenceServiceRepair
  },
  data() {
    return {
      viewingDetails: false,
      viewingSetServiceDetails: false,
      selectedOccurence: {},
      filter: '',
      statusOptions: ['Open', 'Closed', 'Canceled', 'Denyed', 'Approved'],
      headers: [
        { text: 'Policy Number', value: 'policyNumber' },
        { text: 'Product Description', value: 'productDescription' },
        { text: 'Occurence Status', value: 'occurenceStatus' },
        { text: 'Actions', value: 'actions', sortable: false }
      ],
      occurences: [
        {
          id: 123,
          policyNumber: '12345',
          productDescription: 'Car Insurance',
          occurenceStatus: 'Open'
        },
        {
          id: 456,
          policyNumber: '67890',
          productDescription: 'Home Insurance',
          occurenceStatus: 'Closed'
        },
        {
          id: 789,
          policyNumber: 'ABC123',
          productDescription: 'Pet Insurance',
          occurenceStatus: 'Canceled'
        },
        {
          id: 1,
          policyNumber: 'ABC456',
          productDescription: 'Home Insurance',
          occurenceStatus: 'Denyed'
        },
        {
          id: 2,
          policyNumber: 'ABC789',
          productDescription: 'Health Insurance',
          occurenceStatus: 'Approved'
        }
      ]
    }
  },
  computed: {
    filteredOccurences() {
      if (this.filter) {
        return this.occurences.filter(occurence => occurence.occurenceStatus === this.filter)
      }
      return this.occurences
    }
  },
  methods: {
    cancelOccurence(item) {
      console.log(`Canceling occurence with policy number: ${item.policyNumber}`)
    },
    async fetchListOfOccurences(policyId) {
      this.$axios.$get("/api/clients/"+this.$auth.user.nif+"/"+policyId+"/occurrences", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.policies = response;
        });
    },
    viewDetails(item) {
      this.selectedOccurence = item.id
      this.viewingDetails = true
      this.viewingSetServiceDetails = false
    },
    viewServiceRepair(item) {
      this.selectedOccurence = item.id
      this.viewingDetails = false
      this.viewingSetServiceDetails = true
    }
  },
  created() {
    this.fetchClient();
  },
}
</script>
