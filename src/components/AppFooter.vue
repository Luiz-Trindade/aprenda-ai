<template>
  <v-bottom-navigation
    v-model="value"
    color="green"
    grow
  >
    <v-btn>
      <v-icon>mdi-home</v-icon>

      Recents
    </v-btn>

    <v-btn>
      <v-icon>mdi-heart</v-icon>

      Favorites
    </v-btn>

    <v-btn>
      <v-icon>mdi-map-marker</v-icon>

      Nearby
    </v-btn>
  </v-bottom-navigation>

</template>

<script setup>
  import { ref } from 'vue'

  const value = ref(0)
</script>