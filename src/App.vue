<template>
    <div id="app">
        <ScatterPlot></ScatterPlot>
        <hr>
        <BarGraph></BarGraph>
        <hr>
        <ColumnChart></ColumnChart>
    </div>
</template>

<script>
    import ColumnChart from './components/ColumnChart.vue';
    import BarGraph from './components/BarGraph.vue';
    import ScatterPlot from './components/ScatterPlot.vue';

    export default {
        name: 'app',
        components: {
            BarGraph,
            ColumnChart,
            ScatterPlot
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    hr {
        border: 1px solid #efefef;
        margin: 6rem 0;
    }

    .heavy { font: bold 11px sans-serif;  }

    .y {
        letter-spacing: 1px;
    }

    .y.axis, .x.axis {
        stroke-width: 2px;
    }

    .bar {
        fill: lightblue;
        shape-rendering: crispEdges;
    }

    .bar-label {
        fill: black;
        font-size: 12px;
        text-anchor: middle;
    }

    .gridline line {
        stroke: lightgrey;
        stroke-opacity: 0.7;
        shape-rendering: crispEdges;
    }

    .gridline path {
        stroke-width: 0;
    }
</style>
