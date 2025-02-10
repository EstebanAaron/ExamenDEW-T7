<template>
  <div ref="chart" style="width: 100%; height: 100%;"></div>
</template>

<script>
export default {
  name: 'GoogleChart',
  props: {
    data: {
      type: Array,
      required: true
    }
  },
  watch: {
    data: 'drawChart'
  },
  mounted() {
    this.loadGoogleCharts();
  },
  methods: {
    loadGoogleCharts() {
      if (typeof google !== 'undefined' && google.charts) {
        google.charts.load('current', { packages: ['corechart'] });
        google.charts.setOnLoadCallback(this.drawChart);
      } else {
        const script = document.createElement('script');
        script.src = 'https://www.gstatic.com/charts/loader.js';
        script.onload = () => {
          google.charts.load('current', { packages: ['corechart'] });
          google.charts.setOnLoadCallback(this.drawChart);
        };
        document.head.appendChild(script);
      }
    },
    drawChart() {
      if (!this.data || !this.data.length) return;

      const data = google.visualization.arrayToDataTable(this.data);
      const options = {
        title: 'Country Data',
        hAxis: { title: 'Country' },
        vAxis: { title: this.data[0][1] }
      };

      const chart = new google.visualization.ColumnChart(this.$refs.chart);
      chart.draw(data, options);
    }
  }
};
</script>

<style scoped>
/* Add your styles here */
</style>