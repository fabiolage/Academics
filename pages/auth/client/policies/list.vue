<template>
  <v-container class="container pl-0">
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>Policies</v-card-title>
          <v-card-text>
            <v-simple-table>
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
                    <td>{{ policy.code }}</td>
                    <td>{{ policy.insuranceCompany }}</td>
                    <td>{{ policy.type }}</td>
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
      policies: [
        {
          id: 1,
          code: 'XYZ123',
          insuranceCompany: 'Acme Insurance',
          type: 'Home',
          covers: ['Fire', 'Theft']
        },
        {
          id: 2,
          code: 'ABC456',
          insuranceCompany: 'Best Insurance',
          type: 'CellPhone',
          covers: ['Broken grass', 'Theft']
        },
        {
          id: 3,
          code: 'DEF789',
          insuranceCompany: 'Sure Insurance',
          type: 'Health',
          covers: ['Life', 'Injury']
        }
      ]
    }
  }
}
</script>
<style scoped>
.container {
  max-width: none !important;
  width: 100% !important;
}
</style>
