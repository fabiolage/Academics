<template>
  <v-card class="mb-5">
    <div class="row">
      <div class="col-9">
        <v-card-title>Ticket Details</v-card-title>
      </div>
      <div class="col-3">
        <v-card-actions class="float-right">
          <v-btn color="error" @click="closeView()">Close</v-btn>
        </v-card-actions>
      </div>
    </div>
    <v-card-text class="mt-3">
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Policy Number"
            v-model="ticket.policy_number"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Occurrence Status"
            v-model="ticket.occurrenceState"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Product Description"
            v-model="ticket.insured_object"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-subheader>Covers</v-subheader>
          <v-chip v-for="cover in ticket.covers" :key="cover" class="ma-2">
            {{ cover }}
          </v-chip>
        </v-col>
        <v-col cols="12">
          <v-textarea
            label="Description"
            v-model="ticket.description"
            readonly
          ></v-textarea>
        </v-col>
        <v-col cols="12" v-if="ticket.attachments && ticket.attachments.length > 0">
          <v-subheader>Attachments</v-subheader>
          <v-list>
            <v-list-item v-for="attachment in ticket.attachments" :key="attachment.id">
              <v-list-item-title>{{ attachment.fileName }}</v-list-item-title>
              <v-btn color="primary" target="_blank" :href="attachment.url">View</v-btn>
            </v-list-item>
          </v-list>
        </v-col>
      </v-row>
    </v-card-text>
    <v-divider></v-divider>
    <div class="row">
      <v-card-title class="ml-3">Review</v-card-title>
    </div>
    <v-card-text>
      <div>
        <v-btn
          @click="
            showModal = true;
            approvedStatus = true;
          "
          color="success"
          >Approve</v-btn
        >
        <v-btn
          @click="
            showModal = true;
            denyedStatus = true;
          "
          color="error"
          >Deny</v-btn
        >

        <v-bottom-sheet v-model="showModal">
          <v-card>
            <v-card-title class="headline">Are you sure?</v-card-title>
            <v-card-actions>
              <v-btn @click="confirmAction" color="success">Confirm</v-btn>
              <v-btn @click="showModal = false" color="error">Cancel</v-btn>
            </v-card-actions>
          </v-card>
        </v-bottom-sheet>
      </div>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  props: ["id"],
  data() {
    return {
      ticket: {},
      showModal: false,
      approvedStatus: false,
      denyedStatus: false
    };
  },
  mounted() {
    this.fetchTicket();
  },
  methods: {
    async fetchTicket() {
      try {
        this.$axios.$get("/api/occurrence/"+this.id, {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.ticket = response;
        });
      } catch (error) {
        console.error(error);
      }
    },
    closeView() {
      this.$emit("close");
    },
    async approveTicket() {
      this.setOccurrence("accepted")
    },
    async denyTicket() {
      this.setOccurrence("rejected")
    },
    confirmAction() {
      this.showModal = false;
      if (this.approvedStatus) {
        this.approveTicket();
      }
      if (this.denyedStatus) {
        this.denyTicket();
      }
      this.approvedStatus = false;
      this.denyedStatus = false;
    },
    setOccurrence(status) {
      this.$axios
        .$put(
          "/api/occurrence/"+ this.id,
          {
            occurrenceState: status,
          },
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.$toast.success('Occurrence '+status+' with success!', { duration: 3000 });
          this.closeView();
        });
    }
  },
};
</script>
