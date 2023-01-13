<template>
  <div>
    <v-list>
      <v-list-item class="px-2">
        <v-list-item-avatar>
          <v-img
            src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTucaZx_5ilAhuxxb9dw268yscUKX_ZQivmXqkoS-E&s"
          ></v-img>
        </v-list-item-avatar>
      </v-list-item>

      <v-list-item link>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            {{ this.$auth.user.name }}
          </v-list-item-title>
          <v-list-item-subtitle>{{ this.$auth.user.email }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>
    <v-divider></v-divider>
  </div>
</template>
export default {}
