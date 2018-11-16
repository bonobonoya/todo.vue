<template>
  <v-container>
    <add-core></add-core>
    <show-core></show-core>
  </v-container>
</template>

<script>
import addCore from './addCore'
import showCore from './showCore'

export default {
  components: {
    addCore,
    showCore
  }
}
</script>

