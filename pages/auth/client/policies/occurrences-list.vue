<template>
  <div class="w-100 mr-4">
    <v-card-actions class="float-right">
      <v-btn @click="filter = ''">Remove Filter</v-btn>
      <v-btn color="error" @click="closeView()">Go Back</v-btn>
    </v-card-actions>
    <occurence-details
      v-if="viewingDetails"
      :id="selectedOccurrenceId"
      @close="closeDetails()"
    />
    <occurence-service-repair
      v-if="viewingSetServiceDetails"
      :id="selectedOccurrenceId"
      @close="viewingSetServiceDetails = false"
    />
    <v-select v-model="filter" :items="statusOptions" label="Filter by status"></v-select>
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
        <v-btn
          v-if="item.occurrenceState == 'opened'"
          color="error"
          @click="cancelOccurence(item.id)"
          >Cancel Occurrence</v-btn
        >
        <v-btn
          v-if="item.occurrenceState == 'accepted'"
          color="normal"
          @click="viewServiceRepair(item.id)"
          >Set service repair</v-btn
        >
      </template>
    </v-data-table>
  </div>
</template>

<script>
import OccurenceDetails from "../occurrences/details.vue";
import OccurenceServiceRepair from "../occurrences/serviceRepair.vue";
export default {
  components: {
    OccurenceDetails,
    OccurenceServiceRepair,
  },
  props: ["id"],
  data() {
    return {
      viewingDetails: false,
      viewingSetServiceDetails: false,
      selectedOccurrenceId: '',
      filter: "",
      statusOptions: [
        "opened",
        "in_analysis",
        "accepted",
        "in_repair",
        "repaired",
        "delivered",
        "rejected",
        "closed",
        "cancelled",
      ],
      headers: [
        { text: "Policy Number", value: "policyNumber" },
        { text: "Description", value: "description" },
        { text: "Occurrence Status", value: "occurrenceState" },
        { text: "Actions", value: "actions", sortable: false },
      ],
      occurrences: [],
    };
  },
  computed: {
    filteredOccurrences() {
      if (this.filter) {
        return this.occurrences.filter(
          (occurrence) => occurrence.occurrenceState === this.filter
        );
      }
      return this.occurrences;
    },
  },
  methods: {
    async cancelOccurence(item) {
      this.$axios
        .$put(
          "/api/occurrence/"+ item,
          {
            occurrenceState: "cancelled",
          },
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.$toast.success('Occurrence cancelled with success!', { duration: 3000 });
          this.fetchListOfOccurences();
        });
    },
    async fetchListOfOccurences() {
      this.$axios
        .$get("/api/clients/" + this.$auth.user.nif + "/" + this.id + "/occurrences", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.occurrences = response;
        });
    },
    closeDetails() {
      this.viewingDetails = false;
      this.fetchListOfOccurences();
    },
    viewDetails(item) {
      this.selectedOccurrenceId = item.id;
      this.viewingDetails = true;
      this.viewingSetServiceDetails = false;
    },
    viewServiceRepair(item) {
      this.selectedOccurrenceId = item;
      this.viewingDetails = false;
      this.viewingSetServiceDetails = true;
    },
    closeView() {
      this.$emit("close");
    },
  },
  created() {
    this.fetchListOfOccurences();
  },
};
</script>
