<template>
  <v-container class="container">
    <ticket-details
      v-if="viewingDetails"
      :id="selectedOccurrenceId"
      @close="onDetailsClosed()"
      class="detail"
    />
    <div class="row">
      <div class="col-8">
        <h3>List of Occurrences</h3>
      </div>
      <div class="col-4">
        <v-btn class="float-right" @click="toggleFilter">{{ filterOpen ? "Show All" : "Show Open" }}</v-btn>
      </div>
    </div>

    <v-data-table
      :headers="headers"
      :items="filteredOccurrences"
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
      <template v-slot:item.occurrenceState="{ item }">
        {{ item.occurrenceState }}
      </template>
      <template v-slot:item.actions="{ item }">
        <v-btn v-if="item.occurrenceState === 'opened'" @click="viewTicket(item.id)"
          >Review</v-btn
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
      selectedOccurrenceId: '',
      loading: false,
      filterOpen: true,
      occurrences: [],
      headers: [
        { text: "Policy Number", value: "policyNumber" },
        { text: "Description", value: "description" },
        { text: "Status", value: "occurrenceState" },
        { text: "Actions", value: "actions" },
      ],
    };
  },
  computed: {
    filteredOccurrences() {
      return this.filterOpen
        ? this.occurrences.filter((occurence) => occurence.occurrenceState === "opened")
        : this.occurrences;
    },
  },
  methods: {
    toggleFilter() {
      this.filterOpen = !this.filterOpen;
    },
    viewTicket(id) {
      this.selectedOccurrenceId = id;
      this.viewingDetails = true;
    },
    async fetchOccurrencesForExpert() {
      this.$axios
        .$get("/api/expert/" + this.$auth.user.nif +"/occurrences", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.occurrences = response;
        });
    },
    onDetailsClosed() {
      this.viewingDetails = false;
      this.selectedOccurrenceId = '';
      this.fetchOccurrencesForExpert();
    }
  },
  created() {
    this.fetchOccurrencesForExpert();
  }
};
</script>
<style scoped>
.container {
  max-width: none !important;
  width: 100% !important;
}
</style>
