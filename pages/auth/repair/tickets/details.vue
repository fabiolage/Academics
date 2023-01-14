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
            <v-list-item v-for="attachment in ticket.documents">
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
      <div class="w-100">
        <v-btn @click="validate()" color="success">Complete repair</v-btn>
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
      files: [],
      completeRepairStatus: true
    };
  },
  mounted() {
    this.fetchTicket();
  },
  methods: {
    onFileChange(event) {
      this.files = event.target.files;
    },
    removeFile(index) {
      this.files.splice(index, 1);
    },
    async fetchTicket() {
      try {
        this.$axios
          .$get("/api/occurrence/" + this.id, {
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
    async completeRepair() {
      this.saveDocs();
      this.$axios
        .$put(
          "/api/occurrence/" + this.id,
          {
            occurrenceState: "repaired",
          },
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.$toast.success("Repair was completed!", { duration: 3000 });
          this.closeView();
        });
    },
    saveDocs() {
      this.files.forEach((file) => {
        this.$axios
        .$post("/api/documents/" + this.id, this.formData(file), {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        });
      });
    },
    confirmAction() {
      this.showModal = false;
      if (this.completeRepairStatus) {
        this.completeRepair();
      }
      this.completeRepairStatus = false;
    },
    openNewFile(file) {
      window.open(URL.createObjectURL(file));
    },
    async openFile(file) {
      this.$axios
        .$get("/api/documents/download/" + file, {
          responseType: "blob",
        })
        .then((response) => {
          const url = window.URL.createObjectURL(response);
          const link = document.createElement("a");
          link.href = url;
          link.target = "_blank";
          document.body.appendChild(link);
          link.click();
        });
    },
    validate() {
      if (this.files.length === 0) {
        this.$toasted.error("Files are missing", {
          duration: 3000,
        });
        return;
      }
      this.showModal = true;
      this.completeRepairStatus = true;
    },
    formData(file) {
      let formData = new FormData()
      formData.append('username', this.$auth.user.nif)
      if (file) {
        formData.append('file', file)
      }
      return formData
    },

  },
};
</script>
<style scoped>
.buttons-row {
  display: flex;
  flex-direction: row;
}
</style>
