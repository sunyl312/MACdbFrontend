<template>
  <span>
        <v-menu

            :offset-y=" offset == 'y'?true:false"
            :offset-x=" offset == 'y'?false:true"
            open-on-hover
        >
      <template v-slot:activator="{ on, attrs }">
<!--      <v-btn color="white" elevation="0" >-->
        <v-icon  v-bind="attrs" :color="iconColor" small v-on="on"  >  mdi-download</v-icon>
<!--      </v-btn>-->
      </template>

      <v-list>
        <v-list-item :href="`/macdb/${endnote}`" target="_blank" >

          <v-list-item-title class="text-left">
            <v-icon :color="$store.state.mainColor" x-small>mdi-format-quote-open-outline</v-icon>
            Endnote
          </v-list-item-title>

        </v-list-item>
        <v-list-item :href="`/macdb/${bib}`" target="_blank">
          <v-list-item-title class="text-left">
            <v-icon :color="$store.state.mainColor" x-small>mdi-format-quote-open-outline</v-icon>
            Bib Tex
          </v-list-item-title>

        </v-list-item>
      </v-list>
    </v-menu>

  </span>
</template>

<script>
export default {
  name: "citationDownload",
  props: ['endnote', 'bib','offset','iconColor'],
}
</script>

<style scoped>

</style>