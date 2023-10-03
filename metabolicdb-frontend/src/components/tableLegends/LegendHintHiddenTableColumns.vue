<template>
  <v-row align="center" no-gutters style="display: block">
    <v-col class="text-left">
      <v-btn fab x-small rounded elevation="0" icon  outlined color="grey" class="text-left font-weight-bold"><v-icon color="black"  small>mdi-dots-grid</v-icon></v-btn>
      Select columns (including hidden columns of the table ) to display.
    </v-col>
    <v-spacer></v-spacer>
  </v-row>
</template>

<script>
export default {
  name: "LegendHintHiddenTableColumns"
}
</script>

<style scoped>

</style>