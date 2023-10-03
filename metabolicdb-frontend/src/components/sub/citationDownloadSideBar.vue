<template>
  <span>
        <v-menu
            left
            offset-x
            open-on-hover
        >
      <template v-slot:activator="{ on, attrs }">
<!--      <v-btn color="white" elevation="0" >-->
        <v-icon :color="iconColor" small v-bind="attrs" v-on="on">  mdi-format-quote-open-outline</v-icon>
        <!--      </v-btn>-->
      </template>
          <v-sheet class="pa-4" color="white" max-width="300" rounded>
            <v-row>
              <v-col>
                <v-sheet class="text-left text-h6">How to Cite:</v-sheet>
                <v-list class="overflow-y-auto">
                  <v-list-item>
                    <v-sheet class="text-left mb-2 mt-4" rounded>
                      <v-icon>mdi-format-quote-open-outline</v-icon>
                      <span>
                        MACdb: A curated knowledgebase for metabolic associations across human cancers. Molecular Cancer Res 2023, MCR-22-0909, [PMID=<a
                          href="https://pubmed.ncbi.nlm.nih.gov/37027007/" target="_blank" style="text-decoration: none">37027007</a>] [DOI=<a
                          href="https://doi.org/10.1158/1541-7786.MCR-22-0909" target="_blank" style="text-decoration: none">10.1158/1541-7786.MCR-22-0909</a>]
                      </span>
                      <v-sheet class="black--text">
                        <a :href="`/macdb/${macdbendnote}`"
                           style="text-decoration: none;color: black"
                           target="_blank">
                              <v-icon :color="$store.state.mainColor" x-small>mdi-download</v-icon>
                              Endnote
                        </a>
                        <a :href="`/macdb/${macdbbib}`" style="text-decoration: none;color: black"
                           target="_blank">
                              <v-icon :color="$store.state.mainColor" x-small>mdi-download</v-icon>
                              Bib Tex
                        </a>
                      </v-sheet>

                      <v-icon>mdi-format-quote-open-outline</v-icon>
                      <span> Database Resources of the National Genomics Data Center, China National Center for Bioinformation in 2022. Nucleic Acids Res 2022, 50(D1):D27-D38. [PMID=<a
                          :style="{color:hilightColor,textDecoration:'none'}"
                          href="https://pubmed.ncbi.nlm.nih.gov/34718731/"
                          target="_blank">34718731</a>][DOI=<a
                          :style="{color:hilightColor,textDecoration:'none'}"
                          href="https://doi.org/10.1093/nar/gkab951"
                          target="_blank">10.1093/nar/gkab951</a>]
                      </span>
                      <v-sheet class="black--text">
                        <a :href="`/macdb/${endnote}`"
                           style="text-decoration: none;color: black"
                           target="_blank">
                              <v-icon :color="$store.state.mainColor" x-small>mdi-download</v-icon>
                              Endnote
                        </a>
                        <a :href="`/macdb/${bib}`" style="text-decoration: none;color: black"
                           target="_blank">
                              <v-icon :color="$store.state.mainColor" x-small>mdi-download</v-icon>
                              Bib Tex
                        </a>
                      </v-sheet>
                    </v-sheet>
                  </v-list-item>
                </v-list>
              </v-col>
            </v-row>
          </v-sheet>


    </v-menu>

  </span>
</template>

<script>
export default {
  name: "citationDownload",
  props: ['offset', 'iconColor'],
  data() {
    return {
      hilightColor: '#0d8dd2',
      endnote: "static/citation/ngdc_paper_endnote.RIS",
      bib: "static/citation/ngdc_paper_bibtex.bib",
      macdbendnote: "static/citation/macdb.RIS",
      macdbbib: "static/citation/macdb.bib",
    }

  }
}
</script>

<style scoped>

</style>