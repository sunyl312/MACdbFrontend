<template>
  <div>
    <v-sheet
        v-show="isok == true"
        class="mt-16 overflow-x-auto"
        height="550"

    >


      <tools-enrichment-common-echarts :options="chartOption"
                                       chartid="enrichment_trait_id"></tools-enrichment-common-echarts>
    </v-sheet>

    <v-sheet v-show="isok == true">
      <vxe-toolbar custom export></vxe-toolbar>

      <vxe-table
          :align="null"
          :column-config="{resizable: true}"
          :data="dat"
          :export-config="{}"
          :loading="tableLoading"
          :print-config="{}"
          border
          empty-text="Empty"
          max-height="500"
          round
          stripe
      >
        <vxe-column field="trait_ontology" sortable title="Trait" width="450">
          <template #default="{row}">
            <a class="font-weight-bold text-body-2" style="text-decoration: none"
               @click="$commonfunc.openAtNewPageTraitDetail(row.trait_ontology_id)"
            >{{ row.trait_ontology }}</a>

          </template>
        </vxe-column>

        <vxe-column field="hits_data" title="Metabolites Hits" type="expand">
          <template #content="{row}">
            <v-sheet class="text-left pa-3" min-width="100">
              <v-chip v-for="item in row.hits_data " :key="item" :color="$store.state.mainColor" class="ma-1"
                      label outlined small @click="$commonfunc.openAtNewPageMetaboliteDetail(item)">
                {{ item }}
              </v-chip>
              <!--            {{row.hits_data}}-->
            </v-sheet>
          </template>
        </vxe-column>
        <vxe-column field="trait_type" sortable title="Trait Type"></vxe-column>
        <vxe-column field="efo_ontology" sortable title="EFO Ontology"></vxe-column>
        <vxe-column field="EFO_id" sortable title="EFO Ontology ID"></vxe-column>
        <!--        <vxe-column sortable field="odd"  title="Odd Ratio" ></vxe-column>-->
        <vxe-column field="p" sortable title="Pvals"></vxe-column>
        <vxe-column field="adjp" sortable title="Adjust Pvals"></vxe-column>
      </vxe-table>
    </v-sheet>


  </div>
</template>

<script>
import Axios from 'axios'
import ToolsEnrichmentCommonEcharts from "./toolsEnrichmentCommonEcharts";

export default {
  name: "toolsEnrichmentSubTrait",
  components: {ToolsEnrichmentCommonEcharts},
  props: ['isok', "taskid"],
  data() {
    return {
      dat: [],
      pagedData: [],
      chartData: {
        data: [],
        maxDotSize: 0,
        maxPval: 1,
        max_logp:10,
      },
      enrich_type: "enrich_trait",
      tableLoading: false,
      chartOption: null
    }
  },

  beforeMount() {

    if (this.isok == true) {

      Axios.get(baseURL + "/api/tool_enrichment_get_data", {
        params: {
          taskid: this.taskid,
          type: this.enrich_type,
        }
      }).then(res => {

        this.dat = res.data
        this.dat = this.sortData()
        this.datTop = this.dat.slice(0, 20) //图中最多显示多少个结果
        this.chartData.data = this.datTop.map(item => {

          return [item.Cancer_type, item.adjp, item.hits]
        })
        this.chartData.maxDotSize = _.maxBy(this.datTop, "hits")['hits']
        this.chartData.maxPval = _.maxBy(this.datTop, "adjp")['adjp']
        // this.pagination.totalPage = this.dat && this.dat.length || 10
        this.setChartOption(); // 生成作图的配置文件

        // console.log("Enrichment Sub Cancer Running...")

      })
    } else {
      this.chartOption = null
    }
  },

  watch: {
    isok() {

      if (this.isok == true) {

        let max_logp = 10

        Axios.get(baseURL + "/api/tool_enrichment_get_data", {
          params: {
            taskid: this.taskid,
            type: this.enrich_type,
          }
        }).then(res => {

          this.dat = res.data
          this.dat = this.sortData()
          this.datTop = this.dat.slice(0, 20) //图中最多显示多少个结果
          this.chartData.data = this.datTop.map(item => {
            this.chartData.max_logp  = this.chartData.max_logp  > (-1 * Math.log10(item.adjp)) ? this.chartData.max_logp : -1 * Math.log10(item.adjp);
            return [item.trait_ontology, -1 * Math.log10(item.adjp), item.hits, item.adjp]
          })
          // console.log(this.chartData.data)
          this.chartData.maxDotSize = _.maxBy(this.datTop, "hits")['hits']
          this.chartData.maxPval = _.maxBy(this.datTop, "adjp")['adjp']
          // this.pagination.totalPage = this.dat && this.dat.length || 10
          this.setChartOption(); // 生成作图的配置文件

          // console.log("Enrichment Sub Trait Running...")
        })
      } else {

        this.chartOption = null
      }
    },
  },


  methods: {

    setChartOption() {

      let remapping = function (k, inmax, outmin = 5, outmax = 30) {
        let inmin = 0
        return ((k - inmin) * (outmax - outmin) / (inmax - inmin) + outmin)
      }
      let that = this
      this.chartOption = {
        // color:"rgba(12,217,151,0.8)",

        toolbox: {
          feature: {
            saveAsImage: {
              // type:"svg",
              title: "save",
              name: "MACdb_Enrichment_Trait",
              pixelRatio: 4,
            }
          }
        },
        color: ['#61a0a8'],
        grid: {
          // height: '80%',
          top: '5%',
          left: "10%",
          right: "10%",
          bottom: "40%"

        },
        yAxis: {
          name: "Adjust P value(-log10)",
          nameLocation: "middle",
          type: "value",
          min: -0.1,
          max: Math.ceil(this.chartData.max_logp),
          nameGap: 25,
          nameTextStyle: {
            fontSize: 12,
            color: "black",
          },
          axisLabel: {
            color: "black",
          }

        },
        xAxis: {
          name: "Trait",
          nameLocation: "end",
          // nameGap:"10%",
          nameTextStyle: {
            fontSize: 12,
            color: "black",
          },
          inverse: false,
          type: "category",
          axisTick: false,
          splitArea: {
            show: true,
          },
          splitLine: {
            show: true,
          },
          axisLabel: {
            align: "right",
            rotate: 45,
            color: "black",
            // overflow: "break",
            padding: [1, 1, 1, 1],
            fontSize: 10,
            interval: 0,
          },
          overflow: "break"

        },
        tooltip: {
          show: true,
          formatter: function (params) {
            return `<div style="text-align: start" >Trait: &nbsp; ${params.data[0]} </div><div style="text-align: start">Adjust Pval: &nbsp; ${params.data[3]}</div><div style="text-align: start">Hits: &nbsp; ${params.data[2]}</div>`
          }

        },

        series: [
          {
            symbolSize: function (val) {
              return remapping(val[2], that.chartData.maxDotSize);
            },
            data: this.chartData.data,
            type: 'scatter',
            encode: {
              x: [0],
              y: [1],

            },

          }
        ]

      }


    },

    sortData(by = "p", desc = false) {

      return this.dat.sort((x, y) => {

        if (desc == false) {

          return x[by] - y[by]

        } else {
          return y[by] - x[by]
        }

      })

    }

  }


}
</script>

<style scoped>

</style>
