<template>
  <v-form ref="form" class="mr-3">
    <v-card-title>Create occurrence</v-card-title>
    <v-select
      v-model="form.policyNumber"
      :items="occurrencesList"
      :return-object="false"
      item-text="policy_number"
      item-value="policy_number"
      label="Select a policy"
      persistent-hint
      hint="Please select a policy"
      required
    ></v-select>
    <v-textarea
      v-model="form.description"
      label="Description of the occurrence"
      required
    />
    <v-select
      v-model="form.expertNif"
      :items="expertList"
      :return-object="false"
      item-text="name"
      item-value="nif"
      label="Select an expert"
      persistent-hint
      hint="Please select a expert"
      required
    ></v-select>
    <v-file-input
      v-model="files"
      label="Upload Files"
      multiple
      show-size
      @input="previewFiles"
    />
    <template v-for="(file, index) in files" class="mb-3">
      <div class="row my-2">
        <v-card-subtitle class="col-10 pr-2">{{ file.name }}</v-card-subtitle>
        <v-card-actions class="col-2">
          <v-btn @click="openFile(file)" color="primary">View</v-btn>
          <v-btn @click="removeFile(file)" color="error" class="ml-3">Remove</v-btn>
        </v-card-actions>
      </div>
    </template>
    <v-btn @click="submit">Submit</v-btn>
  </v-form>
</template>

<script>
export default {
  data() {
    return {
      form: {
        policyNumber: '',
        description: '',
        expertNif: ''
      },
      valid: false,
      files: [],
      occurrencesList: [],
      expertList: [],
    }
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        this.$axios
        .$post(
          "/api/occurrence",
          this.form,
          {
            headers: {
              Accept: "application/json",
            },
          }
        )
        .then(() => {
          this.$toast.success('Occurrence created with success!', { duration: 3000 });
        });
      }
    },
    previewFiles(files) {
      this.files = files
    },
    removeFile(file) {
      this.files.splice(this.files.indexOf(file), 1)
    },
    openFile(file) {
      window.open(URL.createObjectURL(file));
    },
    async fetchClient() {
      this.$axios.$get("/api/clients/"+this.$auth.user.nif+"/policies", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.occurrencesList = response;
        });
    },
    async fetchExpert() {
      this.$axios.$get("/api/expert", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.expertList = response;
        });
    }
  },
  created() {
    this.fetchClient();
    this.fetchExpert();
  }

}
</script>
