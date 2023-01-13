<template>
  <v-card class="mb-5">
    <div class="row">
      <div class="col-9">
        <v-card-title>Ticket Details</v-card-title>
      </div>
      <div class="col-3">
        <v-card-actions class="float-right">
          <v-btn color="error" @click="closeView">Close</v-btn>
        </v-card-actions>
      </div>
    </div>
    <v-card-text class="mt-3">
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Policy Number"
            v-model="ticket.policyNumber"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Ticket Status"
            v-model="ticket.ticketStatus"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Product Description"
            v-model="ticket.productDescription"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Date and Time of Incident"
            v-model="ticket.incidentDateTime"
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
  props: ["ticketId"],
  data() {
    return {
      ticket: {
        id: 456,
        policyNumber: 12345,
        productDescription: "Home",
        ticketStatus: "Open",
        incidentDateTime: "2023-01-03 18:00",
        description: "Description goes here",
        attachments: [
          {
            id: 1,
            fileName: "File 1",
            url:
              "https://ead.ipleiria.pt/2022-23/pluginfile.php/109374/mod_resource/content/12/DAE-2022-23-1S-ENUNCIADO_PROJETO.pdf",
          },
          {
            id: 2,
            fileName: "File 2",
            url:
              "https://ead.ipleiria.pt/2022-23/pluginfile.php/109374/mod_resource/content/12/DAE-2022-23-1S-ENUNCIADO_PROJETO.pdf",
          },
        ],
        covers: ["Collision", "Liability"],
      },
      message: "",
      showModal: false,
      selectedStore: null,
      stores: [
        { id: 1, name: "Worten" },
        { id: 2, name: "PCDiga" },
        { id: 3, name: "Fnac" },
      ],
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
        // something like this
        //const response = await axios.get(`/api/tickets/${this.ticketId}`)
        //this.ticket = response.data
      } catch (error) {
        console.error(error);
      }
    },
    closeView() {
      this.$emit("close");
    },
    async approveTicket() {
      // TODO
      console.log("approved ticket");
    },
    async denyTicket() {
      // TODO
      console.log("denyed ticket");
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
  },
};
</script>
