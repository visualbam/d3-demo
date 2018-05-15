<template>
    <div id="app">
        <Bargraph></Bargraph>
        <ColumnChart></ColumnChart>
    </div>
</template>

<script>
    import ColumnChart from './components/ColumnChart.vue';
    import Bargraph from './components/Bargraph.vue';

    export default {
        name: 'app',
        components: {
            Bargraph,
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
