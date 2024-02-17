<script>
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import worldData from "./world.json";
  import covidCases from "./country.json";

  onMount(() => {
    renderGlobe(worldData, covidCases);
  });

  function debounce(func, wait, immediate) {
    let timeout;
    return function () {
      const context = this,
        args = arguments;
      const later = function () {
        timeout = null;
        if (!immediate) func.apply(context, args);
      };
      const callNow = immediate && !timeout;
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
      if (callNow) func.apply(context, args);
    };
  }

  // Optimized mouseover handler
  const handleMouseover = debounce(function (event, d) {
    const countryData = covidCases[d.properties.name];
    console.log(countryData);
    const cases = countryData ? countryData["Total Cases"] : 0;
    const population = countryData ? countryData["Population"] : "Unknown";
    const gdp = countryData ? countryData["GDP ($ per capita)"] : "N/A";
    const casesPerHundred = population !== "Unknown" ? ((cases / population) * 100).toFixed(2) : "N/A";
    d3.select("#tooltip")
      .html(
        `Country: ${d.properties.name}<br>GDP ($ per capita): ${gdp}<br>Total Cases: ${cases}<br>Cases per 100 People: ${casesPerHundred}<br>Population: ${population}`
      )
      .style("left", event.pageX + 10 + "px")
      .style("top", event.pageY - 28 + "px")
      .style("visibility", "visible");
  }, 500); // Adjust debounce time as needed

  function renderGlobe(data, covidCases) {
    const mapElement = document.querySelector("#map");
    if (!mapElement) {
      console.error("Map element not found");
      return;
    }
    const width = mapElement.getBoundingClientRect().width;
    const height = 500;
    const sensitivity = 75;

    let projection = d3
      .geoOrthographic()
      .scale(250)
      .center([0, 0])
      .rotate([0, -30])
      .translate([width / 2, height / 2]);

    const initialScale = projection.scale();
    let path = d3.geoPath().projection(projection);

    let svg = d3
      .select("#map")
      .append("svg")
      .attr("width", width)
      .attr("height", height);

    svg
      .append("circle")
      .attr("fill", "#EEE")
      .attr("stroke", "#000")
      .attr("stroke-width", "0.2")
      .attr("cx", width / 2)
      .attr("cy", height / 2)
      .attr("r", initialScale);

    svg
      .call(
        d3.drag().on("drag", (event) => {
          const rotate = projection.rotate();
          const k = sensitivity / projection.scale();
          projection.rotate([
            rotate[0] + event.dx * k,
            rotate[1] - event.dy * k,
          ]);
          path = d3.geoPath().projection(projection);
          svg.selectAll("path").attr("d", path);
        })
      )
      .call(
        d3.zoom().on("zoom", (event) => {
          if (event.transform.k > 0.3) {
            projection.scale(initialScale * event.transform.k);
            path = d3.geoPath().projection(projection);
            svg.selectAll("path").attr("d", path);
            svg.select("circle").attr("r", projection.scale());
          }
        })
      );

    let map = svg.append("g");

    //COVID FUNCTIONALITY
    const casesPerPopulationValues = Object.values(covidCases).map(
      (country) => {
        const cases = country["Total Cases"] || 0;
        const population = country["Population"] || 1;
        return cases / population;
      }
    );
    const maxCasesPerPopulation = d3.max(casesPerPopulationValues);
    const minCasesPerPopulation = d3.min(casesPerPopulationValues);

    const colorScale = d3
      .scaleSequential()
      .domain([minCasesPerPopulation, maxCasesPerPopulation])
      .interpolator(d3.interpolateReds); // Use a different interpolator for better color variation

    map
      .append("g")
      .attr("class", "countries")
      .selectAll("path")
      .data(data.features)
      .enter()
      .append("path")
      .attr("d", path)
      .attr("fill", (d) => {
        const countryData = covidCases[d.properties.name];
        if (!countryData) return "#ccc"; // Fallback color for missing data
        const cases = countryData["Total Cases"] || 0;
        const population = countryData["Population"] || 1;
        const casesPerPopulation = cases / population;
        return colorScale(casesPerPopulation);
      })
      .style("stroke", "black")
      .style("stroke-width", 0.3)
      .style("opacity", 0.8)
      // Add mouseover and mouseout event listeners
      .on("mouseover", handleMouseover)
      .on("mouseout", function () {
        d3.select("#tooltip").style("visibility", "hidden");
      });
    // Optional rotate
    d3.timer(function (elapsed) {
      const rotate = projection.rotate();
      const k = sensitivity / projection.scale();
      projection.rotate([rotate[0] - 0.5 * k, rotate[1]]);
      path = d3.geoPath().projection(projection);
      svg.selectAll("path").attr("d", path);
    }, 200);
  }
</script>

<div id="map" style="width: 100%;"></div>
<div
  id="tooltip"
  style="position: absolute; visibility: hidden; background-color: white; border: 1px solid #000; padding: 10px; border-radius: 5px; pointer-events: none;"
></div>
