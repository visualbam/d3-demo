<template>
    <div id="app">
        <div id="chart-area"></div>
    </div>
</template>

<script>
    import HelloWorld from './components/HelloWorld.vue'
    import * as d3 from 'd3';
    import { revenue } from './mock-data/revenue';

    export default {
        name: 'app',
        components: {
            HelloWorld
        },
        mounted() {
            let margin = { left: 80, right: 20, top: 50, bottom: 100 };

            let width = 850 - margin.left - margin.right,
                height = 400 - margin.top - margin.bottom;

            let g = d3.select('#chart-area')
                .append('svg')
                    .attr('width', width + margin.left + margin.right)
                    .attr('height', height + margin.top + margin.bottom)
                .append('g')
                    .attr('transform', `translate(${margin.left}, ${margin.top})`);

            g.append('text')
                .attr('y', height + 50)
                .attr('x', width / 2)
                .attr('font-size', '20px')
                .attr('text-anchor', 'middle')
                    .text('Month');

            g.append('text')
                .attr('y', - 60)
                .attr('x', - (height / 2))
                .attr('font-size', '20px')
                .attr('text-anchor', 'middle')
                .attr('transform', 'rotate(-90)')
                    .text('Revenue');

            // X Scale
            let x = d3.scaleBand()
                .domain(revenue.map(d => d.month))
                .range([0, width])
                .padding(0.2);

            // Y Scale
            let y = d3.scaleLinear()
                .domain([0, d3.max(revenue, r => r.revenue)])
                .range([height, 0]);

            // X Axis
            let xAxisCall = d3.axisBottom(x);
            g.append('g')
                .attr('class', 'x axis')
                .attr('transform', `translate(0, ${height})`)
                    .call(xAxisCall);

            // Y Axis
            let yAxisCall = d3.axisLeft(y).tickFormat(d => `$${d}`);
            g.append('g')
                .attr('class', 'y axis')
                    .call(yAxisCall);

            let rects = g.selectAll('rect')
                .data(revenue);

            rects.enter()
                .append('rect')
                    .attr('y', d => y(d.revenue))
                    .attr('x', d => x(d.month))
                    .attr('height', d => height - y(d.revenue))
                    .attr('width', x.bandwidth)
                    .attr('fill', 'pink');
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
