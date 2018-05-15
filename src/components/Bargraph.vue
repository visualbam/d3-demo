<template>
    <div class="container">
        <h1>Bargraph</h1>
        <div class="bargraph"></div>
    </div>
</template>

<script>
    /* eslint-disable */
    import * as d3 from 'd3';
    import { donuts } from '../mock-data/donuts';

    /*
        Resources
        ----------------
        List of SVG Elements: https://developer.mozilla.org/en-US/docs/Web/SVG/Element


        Steps
        -----------------
        1. Configure chart/graph container properties
        2. Set xScale and yScale functions to data values
        3. Create SVG elements
            a. The entire chart/graph
            b. Elements that get placed on to the chart/graph
            c. Chart text lables
     */

    export default {
        name: "Bargraph",
        mounted() {
            let data = donuts;

            // Configure chart dimensions statically
            let w = 800;
            let h = 450;

            // Configure chart margins
            let margin = { top: 20, bottom: 20, left: 20, right: 20 };

            // Configure chart dimensions based on data in relation to the margins
            let width = w - margin.left - margin.right;
            let height = h - margin.top - margin.bottom;

            // Data Scale
            let xScale = d3.scaleLinear()
                // min - max values for x axis
                .domain([0, d3.max(data, function(d) {
                    return d.value;
                })])
                // scales the width of the data to the chart's width
                .range([0, width]);

            let yScale = d3.scaleLinear()
                // min - max values for y axis
                .domain([0, data.length])
                // scales the height of the data to the chart's height
                .range([0, height]);

            // SVG Creation
            let svg = d3.select('.bargraph').append('svg')
                .attr('id', 'chart')
                .attr('width', w)
                .attr('height', h);

            // g element groups our SVG so that we can move the entire group of elements
            let chart = svg.append('g')
                .classed('display', true)
                // Transform allows us to move the group with x / y values
                .attr('transform', 'translate(' + margin.left +', ' + margin.top +')');

            // Plot data to chart/graph
            plot.call(chart, { data: data });

            // --------------------------------------------------------------
            function plot(params) {
                // Create bars
                this.selectAll('.bar')
                    .data(params.data)
                    .enter()
                        .append('rect')
                        .classed('bar', true)
                        .attr('x', 0)
                        .attr('y', function(d, i) {
                            return yScale(i);
                        })
                        .attr('width', function(d, i) {
                            return xScale(d.value);
                        })
                        .attr('height', function(d, i) {
                            // scale y data to height of the chart then subtract 2 pixels for space between bars
                            return yScale(1)-2;
                        });

                // Add text labels
                // Move text using dx and dy attributes
                this.selectAll('.bar-label')
                    .data(params.data)
                    .enter()
                        .append('text')
                        .classed('bar-label', true)
                        .attr('x', function(d, i) {
                            return xScale(d.value);
                        })
                        .attr('dx', -4)
                        .attr('y', function(d, i) {
                            return yScale(i);
                        })
                        .attr('dy', function(d, i) {
                            // scales the position to the height of 1 bar divided by 1.5 bar
                            return yScale(1) / 1.5;
                        })
                        .text(function(d, i) {
                            return d.value;
                        });
            }
        }
    }
</script>

<style>
    svg {
        border: 1px solid #efefef;
    }

    .bar {
        fill: lightblue;
        shape-rendering: crispEdges;
    }

    .bar-label {
        fill: white;
        font-size: 12px;
        text-anchor: end;
    }
</style>