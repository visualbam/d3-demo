<template>
    <div class="container">
        <h1>Scatter Plot</h1>
        <div class="scatter-plot"></div>
    </div>
</template>

<script>
    /* eslint-disable */
    import * as d3 from 'd3';
    import { investments } from '../mock-data/investments';

    /*
    * x-axis: STANDARD DEVIATION 3 YR %
    * ------------------------------------
    * 1. 3YrAnnualized
    * 2. 5YrAnnualized
    * */

    /*
    * y-axis: RETURN 3 YR %
    * ------------------------------------
    * 1. Model monthly returns by hypothetical or historical
    * 2. Peer group benchmark returns
    * 3. Blended benchmark returns
    * */

    export default {
        name: "ScatterPlot",
        mounted() {
            let data = investments;

            // Chart dimensions ----------------------------------------------------------------------------------------

            let w = 800;
            let h = 250;

            // Configure chart margins ---------------------------------------------------------------------------------

            let margin = { top: 20, bottom: 55, left: 60, right: 20 };

            // Configure chart dimensions based on data in relation to the margins -------------------------------------

            let width = w - margin.left - margin.right;
            let height = h - margin.top - margin.bottom;

            // Create Chart Scale --------------------------------------------------------------------------------------

            let xScale = d3.scaleLinear()
                .domain([0, d3.max(data, function(d) { return d.StandardDeviatrionThreeYear; })])
                .range([0, width])
                .nice();

            let yScale = d3.scaleLinear()
                .domain([0, d3.max(data, function(d) { return d.ThreeYearReturn })])
                .range([height, 0])
                .nice();

            // Create Color Scale --------------------------------------------------------------------------------------

            let linearColorScale = d3.scaleLinear()
                .domain([0, data.length])
                .range(['#572500', '#f68026']);

            let ordinalColorScale = d3.scaleOrdinal(d3.schemeCategory10);

            // Create Axis ---------------------------------------------------------------------------------------------
            let formatAxis = d3.format('.0f');
            let xAxis = d3.axisBottom(xScale).tickSize(0).tickPadding(15).tickFormat(formatAxis).ticks(10);
            let yAxis = d3.axisLeft(yScale).tickSize(0).tickPadding(15).tickFormat(formatAxis).ticks(7);

            // Create Grid lines ---------------------------------------------------------------------------------------

            let yGridLines = d3.axisLeft(yScale)
                .tickSize(-width, 0, 0)
                .tickFormat('')
                .ticks(7);

            let xGridLines = d3.axisBottom(xScale)
                .tickSize(-height, 0, 0)
                .tickFormat('')
                .ticks(10);

            // SVG Creation --------------------------------------------------------------------------------------------

            let svg = d3.select('.scatter-plot').append('svg')
                .attr('id', 'chart')
                .attr('width', w)
                .attr('height', h);

            let chart = svg.append('g')
                .classed('display', true)
                .attr('transform', 'translate(' + margin.left + ', ' + margin.top + ')');

            // Plot Chart ----------------------------------------------------------------------------------------------

            plot.call(chart, {
                data: data,
                scale: {
                    x: xScale,
                    y: yScale
                },
                axis: {
                    x: xAxis,
                    y: yAxis
                },
                gridLines: {
                    x: xGridLines,
                    y: yGridLines
                }
            });

            function ColorLuminance(hex, lum) {
                // validate hex string
                hex = String(hex).replace(/[^0-9a-f]/gi, '');
                if (hex.length < 6) { hex = hex[0]+hex[0]+hex[1]+hex[1]+hex[2]+hex[2]; }
                lum = lum || 0;

                // convert to decimal and change luminosity
                var rgb = "#", c, i;
                for (i = 0; i < 3; i++) {
                    c = parseInt(hex.substr(i*2,2), 16);
                    c = Math.round(Math.min(Math.max(0, c + (c * lum)), 255)).toString(16);
                    rgb += ("00"+c).substr(c.length);
                }

                return rgb;
            }

            function plot(params) {
                // enter()
                this.selectAll('.point')
                    .data(params.data)
                    .enter()
                        .append('circle')
                        .classed('point', true);

                // update()
                this.selectAll('.point')
                    .attr('r', 10)
                    .attr('fill', function(d) {
                        return d.Color;
                    })
                    .attr('stroke-width', 1)
                    .attr('stroke', function(d) {
                        return ColorLuminance(d.Color, -0.25);
                    })
                    .attr('cx', function(d) {
                        return params.scale.x(d.StandardDeviatrionThreeYear);
                    })
                    .attr('cy', function(d) {
                        return params.scale.y(d.ThreeYearReturn);
                    });

                // exit()
                this.selectAll('.point')
                    .data(params.data)
                    .exit()
                    .remove();

                this.append('g')
                    .call(params.gridLines.y)
                    .classed('gridline', true);

                this.append('g')
                    .attr('transform', 'translate(0,' + height + ')')
                    .call(params.gridLines.x)
                    .classed('gridline', true);

                this.append('g')
                    .classed('x axis', true)
                    .classed('heavy', true)
                    .attr('transform', 'translate(' + 0 + ', ' + height + ')')
                    .call(params.axis.x);

                this.append('g')
                    .classed('y axis', true)
                    .classed('heavy', true)
                    .call(params.axis.y);

                this.select('.y.axis')
                    .append('text')
                    .attr('x', 0)
                    .attr('y', -10)
                    .classed('heavy', true)
                    .attr('text-anchor', 'middle')
                    .attr('fill', 'black')
                    // Translate label to be positioned at half the height of the chart (middle) & rotate 90 degrees
                    .attr('transform', 'translate(-30, ' + height / 2 + ') rotate(-90)')
                    .text('RETURN 3 YR %');

                this.select('.x.axis')
                    .append('text')
                    .attr('x', 0)
                    .attr('y', 0)
                    .classed('heavy', true)
                    .attr('text-anchor', 'middle')
                    .attr('transform', 'translate(' + width / 2 + ', 45)')
                    .attr('fill', 'black')
                    .text('STANDARD DEVIATION 3 YR %')
            }
        }
    }
</script>

<style>

</style>