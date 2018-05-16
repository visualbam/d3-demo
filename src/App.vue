<template>
    <div id="app">
        <BarGraph></BarGraph>
        <ColumnChart></ColumnChart>
    </div>
</template>

<script>
    import ColumnChart from './components/ColumnChart.vue';
    import BarGraph from './components/BarGraph.vue';

    export default {
        name: 'app',
        components: {
            BarGraph,
            ColumnChart
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
</style>
