<template>
  <v-form ref="form">
    <v-card-title>Create ticket</v-card-title>
    <v-text-field
      v-model="policyNumber"
      label="Policy Number"
      required
    />
    <div class="row">
      <v-text-field
        class="col-6"
        type="date"
        v-model="accidentDate"
        label="Date of Accident"
        :error-messages="accidentDateErrors"
        @input="validateAccidentDate"
        required
      />
      <v-text-field
        class="col-6"
        type="time"
        v-model="accidentTime"
        label="Time of Accident"
        :error-messages="accidentTimeErrors"
        @input="validateAccidentTime"
        required
      />
    </div>
    <v-textarea
      v-model="accidentDescription"
      label="Description of Accident"
      required
    />
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
import moment from "moment";
export default {
  data() {
    return {
      policyNumber: '',
      accidentDate: '',
      accidentTime: '',
      accidentDescription: '',
      accidentDateErrors: [],
      accidentTimeErrors: [],
      valid: false,
      files: []
    }
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        // submit form
      }
    },
    validateAccidentDate() {
      const currentDate = moment().format('YYYY-MM-DD')
      if (this.accidentDate > currentDate) {
        this.accidentDateErrors = ['Accident date cannot be in the the future'];
      } else {
        this.accidentDateErrors = []
      }
    },
    validateAccidentTime() {
      const currentDate = moment().format('YYYY-MM-DD')
      const currentTime = moment().format('HH:mm')
      if ((moment(this.accidentDate).isSame(currentDate)) && (this.accidentTime > currentTime)) {
        this.accidentTimeErrors = ['Accident time cannot be in the the future']
      } else {
        this.accidentTimeErrors = []
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
    }
  }
}
</script>
