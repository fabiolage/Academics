<template>
  <v-card>
    <div class="row w-90">
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
            v-model="ticket.policy_number"
            readonly
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Ticket Status"
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
  </v-card>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      ticket: {}
    }
  },
  mounted() {
    this.fetchTicket()
  },
  methods: {
    async fetchTicket() {
      this.$axios.$get("/api/occurrence/"+this.id, {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.ticket = response;
        });
    },
    closeView() {
      this.$emit('close')
    }
  }
}
</script>
