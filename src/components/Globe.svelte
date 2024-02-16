<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import worldData from './world.json'; // Ensure this path is correct
  
    onMount(() => {
      renderGlobe(worldData);
    });
  
    function renderGlobe(data) {
      const mapElement = document.querySelector("#map");
      if (!mapElement) {
        console.error('Map element not found');
        return;
      }
      const width = mapElement.getBoundingClientRect().width;
      const height = 500;
      const sensitivity = 75;
  
      let projection = d3.geoOrthographic()
        .scale(250)
        .center([0, 0])
        .rotate([0, -30])
        .translate([width / 2, height / 2]);
  
      const initialScale = projection.scale();
      let path = d3.geoPath().projection(projection);
  
      let svg = d3.select("#map")
        .append("svg")
        .attr("width", width)
        .attr("height", height);
  
      svg.append("circle")
        .attr("fill", "#EEE")
        .attr("stroke", "#000")
        .attr("stroke-width", "0.2")
        .attr("cx", width / 2)
        .attr("cy", height / 2)
        .attr("r", initialScale);
  
      svg.call(d3.drag().on('drag', (event) => {
        const rotate = projection.rotate();
        const k = sensitivity / projection.scale();
        projection.rotate([
          rotate[0] + event.dx * k,
          rotate[1] - event.dy * k
        ]);
        path = d3.geoPath().projection(projection);
        svg.selectAll("path").attr("d", path);
      }))
      .call(d3.zoom().on('zoom', (event) => {
        if(event.transform.k > 0.3) {
          projection.scale(initialScale * event.transform.k);
          path = d3.geoPath().projection(projection);
          svg.selectAll("path").attr("d", path);
          svg.select("circle").attr("r", projection.scale());
        }
      }));
  
      let map = svg.append("g");


      //COVID FUNCTIONALITY
      const colorScale = d3.scaleSequentialLog()
        .domain([1, d3.max(Object.values(covidCases))])
        .range(["green", "orange", "red"]);

  
      map.append("g")
        .attr("class", "countries")
        .selectAll("path")
        .data(data.features)
        .enter().append("path")
        .attr("d", path)
        // Set fill based on COVID-19 cases
        .attr("fill", d => {
        const cases = covidCases[d.id] || 0; // Replace d.id with the correct identifier
        return colorScale(cases);
        })
        .style('stroke', 'black')
        .style('stroke-width', 0.3)
        .style("opacity", 0.8);
  
      // Optional rotate
      d3.timer(function(elapsed) {
        const rotate = projection.rotate();
        const k = sensitivity / projection.scale();
        projection.rotate([
          rotate[0] - 1 * k,
          rotate[1]
        ]);
        path = d3.geoPath().projection(projection);
        svg.selectAll("path").attr("d", path);
      }, 200);
    }
  </script>
  
  <div id="map" style="width: 100%;"></div>