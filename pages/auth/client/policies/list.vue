<template>
  <v-container class="container pl-0">
    <v-row v-if="!showListOfOccurences">
      <v-col cols="12">
        <v-card>
          <v-card-title>Policies</v-card-title>
          <v-card-text>
            <v-simple-table v-if="policies.length > 0">
              <template v-slot:default>
                <thead>
                  <tr>
                    <th id="policy-code" class="text-left">Code</th>
                    <th id="policy-insure-company" class="text-left">
                      Insurance Company
                    </th>
                    <th id="policy-type" class="text-left">Type</th>
                    <th id="policy-coverage" class="text-left">Coverage</th>
                    <th id="policy-actions"></th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="policy in policies" :key="policy.id">
                    <td>{{ policy.policy_number }}</td>
                    <td>{{ policy.company_name }}</td>
                    <td>{{ policy.insured_object }}</td>
                    <td>
                      <v-chip
                        v-for="cover in policy.covers"
                        :key="cover"
                        color="primary"
                        text-color="white"
                        class="ma-2"
                      >
                        {{ cover }}
                      </v-chip>
                    </td>
                    <td>
                      <v-btn color="primary" @click="viewOccurances(policy.policy_number)">View Occurences</v-btn>
                    </td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-if="showListOfOccurences && selectedPolicyNumber != ''">
      <occurrences-list :id="selectedPolicyNumber" @close="closeListOfOccurences()"/>
    </v-row>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      policies: [],
      selectedPolicyNumber: '',
      showListOfOccurences: false
    };
  },
  components: {
    "occurrences-list": () => import("@/pages/auth/client/policies/occurrences-list.vue"),
  },
  methods: {
    async fetchClient() {
      this.$axios.$get("/api/clients/"+this.$auth.user.nif+"/policies", {
          headers: {
            Accept: "application/json",
          },
        })
        .then((response) => {
          this.policies = response;
        });
    },
    viewOccurances(policyNumber)  {
      this.selectedPolicyNumber = policyNumber;
      this.showListOfOccurences = true;
    },
    closeListOfOccurences() {
      this.showListOfOccurences = false;
      this.selectedPolicyNumber = '';
    }
  },
  created() {
    this.fetchClient();
  },
};
</script>
<style scoped>
.container {
  max-width: none !important;
  width: 100% !important;
}
</style>
