<template>
  <v-card class="mb-5">
    <div class="row">
      <div class="col-9">
        <v-card-title>Ticket Details</v-card-title>
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
        <v-col cols="12" v-if="ticket.documents && ticket.documents.length > 0">
          <v-subheader>Attachments</v-subheader>
          <v-list>
            <v-list-item v-for="attachment in ticket.documents" :key="attachment">
              <v-list-item-title>{{ attachment }}</v-list-item-title>
              <v-btn color="primary" @click="openFile(attachment)">View</v-btn>
            </v-list-item>
          </v-list>
        </v-col>
      </v-row>
    </v-card-text>
    <v-divider></v-divider>
    <div class="row">
      <v-card-title class="ml-3">Repair Information</v-card-title>
    </div>
    <v-card-text>
      <div class="w-100">
        <v-file-input v-model="files" label="Select files" multiple></v-file-input>
        <v-list>
          <v-list-item v-for="file in files" :key="file.id">
            <v-list-item-content>
              <v-list-item-title>{{ file.name }}</v-list-item-title>
            </v-list-item-content>
            <v-list-item-action class="buttons-row">
              <v-btn @click="openNewFile(file)" color="primary">View</v-btn>
              <v-btn class="ml-3" @click="removeFile(file.id)" color="error"
                >Delete</v-btn
              >
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </div>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  middleware: 'auth',
  async asyncData({ params }) {
    return { id: params.id }
  },
  data() {
    return {
      ticket: {},
      files: []
    }
  },
  created() {
    console.log(this.$route.params.id);
      this.$axios.$get("/api/occurrence/"+thiss.id, {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.ticket = response;
        });
  }
}
</script>
