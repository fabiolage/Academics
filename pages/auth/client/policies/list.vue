<template>
  <v-container class="container pl-0">
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>Policies</v-card-title>
          <v-card-text>
            <v-simple-table v-if="policies.length != 0">
              <template v-slot:default>
                <thead>
                  <tr>
                    <th id="policy-code" class="text-left">Code</th>
                    <th id="policy-insure-company" class="text-left">Insurance Company</th>
                    <th id="policy-type" class="text-left">Type</th>
                    <th id="policy-coverage" class="text-left">Coverage</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="policy in policies" :key="policy.id">
                    <td>{{ policy.policy_number }}</td>
                    <td>{{ policy.company_name }}</td>
                    <td>{{ policy.insured_object }}</td>
                    <td>
                      <v-chip v-for="cover in policy.covers" :key="cover" color="primary" text-color="white" class="ma-2">
                        {{ cover }}
                      </v-chip>
                    </td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      policies: []
    }
  },
  methods: {
    async fetchClient() {

      this.$axios
      .$get("clients/997909748", {
        headers: {
          'Accept': 'application/json'
        }
      }).then(response => {this.policies = response})
    }
  },
  created() {
    this.fetchClient();
  }
}

</script>
<style scoped>
.container {
  max-width: none !important;
  width: 100% !important;
}
</style>
