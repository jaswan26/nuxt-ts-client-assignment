<template>
  <div class="hello" ref="chartdiv">
  </div>
</template>
  
<script setup lang="ts">
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
import { onMounted, ref } from "vue";

am4core.useTheme(am4themes_animated);

let chartdiv = ref<HTMLElement | string>("")

onMounted(() => {
  let chart = am4core.create(chartdiv.value, am4charts.XYChart);

  chart.paddingRight = 20;

  let data = [{
    "year": "2016",
    "europe": 2.5,
    "namerica": 2.5,
    "asia": 2.1,
    "lamerica": 0.3,
    "meast": 0.2,
    "africa": 0.1
  }, {
    "year": "2017",
    "europe": 2.6,
    "namerica": 2.7,
    "asia": 2.2,
    "lamerica": 0.3,
    "meast": 0.3,
    "africa": 0.1
  }, {
    "year": "2018",
    "europe": 2.8,
    "namerica": 2.9,
    "asia": 2.4,
    "lamerica": 0.3,
    "meast": 0.3,
    "africa": 0.1
  }];

  chart.data = data;


  let categoryAxis = chart.xAxes.push(new am4charts.CategoryAxis());
  categoryAxis.dataFields.category = "year";
  categoryAxis.renderer.grid.template.location = 0;


  let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
  valueAxis.renderer.inside = true;
  valueAxis.renderer.labels.template.disabled = true;
  valueAxis.min = 0;

  // Create series
  function createSeries(field: string, name: string) {

    // Set up series
    let series = chart.series.push(new am4charts.ColumnSeries());
    series.name = name;
    series.dataFields.valueY = field;
    series.dataFields.categoryX = "year";
    series.sequencedInterpolation = true;

    // Make it stacked
    series.stacked = true;

    // Configure columns
    series.columns.template.width = am4core.percent(60);
    series.columns.template.tooltipText = "[bold]{name}[/]\n[font-size:14px]{categoryX}: {valueY}";

    // Add label
    let labelBullet = series.bullets.push(new am4charts.LabelBullet());
    labelBullet.label.text = "{valueY}";
    labelBullet.locationY = 0.5;
    labelBullet.label.hideOversized = true;

    return series;
  }

  createSeries("europe", "Europe");
  createSeries("namerica", "North America");
  createSeries("asia", "Asia-Pacific");
  createSeries("lamerica", "Latin America");
  createSeries("meast", "Middle-East");
  createSeries("africa", "Africa");


})
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  width: 100%;
  height: 500px;
}
</style>
