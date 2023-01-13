<template>
  <div class="w-100 mr-4">
    <v-card-actions class="float-right">
      <v-btn @click="filter = ''">Remove Filter</v-btn>
      <v-btn color="error" @click="closeView()">Go Back</v-btn>
    </v-card-actions>
    <occurence-details v-if="viewingDetails" :occurence="selectedOccurence" @close="viewingDetails = false" />
    <occurence-service-repair v-if="viewingSetServiceDetails" :occurence="selectedOccurence" @close="viewingSetServiceDetails = false" />
    <v-select
      v-model="filter"
      :items="statusOptions"
      label="Filter by status"
    ></v-select>
    <v-data-table
      :headers="headers"
      :items="filteredOccurrences"
      class="elevation-1"
      v-if="filteredOccurrences.length > 0"
    >
      <template v-slot:item.policyNumber="{ item }">
        {{ item.policyNumber }}
      </template>
      <template v-slot:item.description="{ item }">
        {{ item.description }}
      </template>
      <template v-slot:item.occurrenceState="{ item }">
        {{ item.occurrenceState }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-btn color="primary" @click="viewDetails(item)">View Details</v-btn>
        <v-btn v-if="item.occurrenceState =='opened'" color="error" @click="cancelOccurence(item)">Cancel Occurence</v-btn>
        <v-btn v-if="item.occurrenceState =='Approved'" color="normal" @click="viewServiceRepair(item)">Set service repair</v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import OccurenceDetails from '../occurrences/details.vue'
import OccurenceServiceRepair from '../occurrences/serviceRepair.vue'
export default {
  components: {
    OccurenceDetails,
    OccurenceServiceRepair
  },
  props: ['id'],
  data() {
    return {
      viewingDetails: false,
      viewingSetServiceDetails: false,
      selectedOccurence: {},
      filter: '',
      statusOptions: ['Open', 'Closed', 'Canceled', 'Denyed', 'Approved'],
      headers: [
        { text: 'Policy Number', value: 'policyNumber' },
        { text: 'Description', value: 'description' },
        { text: 'Occurrence Status', value: 'occurrenceState' },
        { text: 'Actions', value: 'actions', sortable: false }
      ],
      occurrences: []
    }
  },
  computed: {
    filteredOccurrences() {
      if (this.filter) {
         return this.occurrences.filter(occurrence => occurrence.occurrenceState === this.filter)
      }
      return this.occurrences
    }
  },
  methods: {
    cancelOccurence(item) {
      console.log(`Canceling occurence with policy number: ${item.policyNumber}`)
    },
    async fetchListOfOccurences(policyId) {
      this.$axios.$get("/api/clients/"+this.$auth.user.nif+"/"+this.id+"/occurrences", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.occurrences = response;
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
    },
    closeView() {
      this.$emit('close')
    }
  },
  created() {
    this.fetchListOfOccurences();
  },
}
</script>
