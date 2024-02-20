<script>
    import Globe from '../components/Globe.svelte'
</script>

<h1 style="text-align: center; font-family: Arial, sans-serif; font-size: 32px; margin-top: 50px;">
    Covid Cases Per Population By Country
</h1>

<main>
    <Globe/>
    
<style>
    .legend {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-top: 20px;
    }
    .legend div {
        width: 20px;
        height: 20px;
        margin: 0 5px;
    }
    .legend .data-not-available {
        background-color: gray;
    }
    .legend .low {
        background-color: #ffcccc;
    }
    .legend .medium {
        background-color: #ff9999;
    }
    .legend .high {
        background-color: #ff6666;
    }
    .legend .very-high {
        background-color: #ff3333;
    }
    .writeup {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.writeup h1, .writeup h2 {
    color: #333;
}

.writeup p, .writeup li {
    color: #666;
    line-height: 1.6;
}

.writeup ul {
    list-style-type: disc;
    margin-left: 20px;
}
</style>

<div class="legend">
    <div class="data-not-available"></div><span>Data Not Available</span>
    <div class="low"></div><span>Low Cases per Population</span>
    <div class="medium"></div><span>Medium Cases per Population</span>
    <div class="high"></div><span>High Cases per Population</span>
    <div class="very-high"></div><span>Very High Cases per Population</span>
</div>

<div class = "writeup">
    <h1>Interactive Visualization of COVID-19 Cases Per Population by Country (2020-2021)</h1>
    <p>After brainstorming on which question we wanted our interactive visualization to answer, we decided on visualizing the COVID-19 cases per population by country for the period of 2020-2021. We knew that visualizing this using an interactive visualization would facilitate the audience’s understanding of the magnitude of COVID-19 across the globe by allowing them to clearly understand the difference in how some countries were affected by COVID-19 more than others.</p>
    <h2>Visualization Design</h2>
    <p>When choosing an effective visualization for this, we wanted to select one that would intuitively illustrate the countries and their corresponding cases per 100 people. We decided on coding an interactive globe that would have as the visual encodings a varying shade of red for each country based on their cases per 100 people. In addition to this, we added a legend right below the interactive globe that includes what proportion of cases each shade of red represents, as well as that gray represents “Data Not Available”. As part of our interaction techniques, we included panning, zooming, and tooltips, as well as an automated constant spin velocity on the globe.</p>
    <h2>Interaction Techniques</h2>
    <p>We chose panning and zooming as our techniques since it was necessary for the user to be able to zoom into specific countries as well as move the frame quickly from one location to another. Lastly, our goal was to add an extra layerof interactivity, so we decided to add tooltips since we wanted the user to be able to know the name of the country, GDP ($ per capita), total cases, cases per 100 people, and population. Solely shading the country in different shades of red wasn’t informative enough, but by adding these tooltips, we ensured that the user would paint a clearer picture of each country’s situation.</p>
    <h2>Design Choices and Teamwork</h2>
    <p>From the beginning, we had it very clear as a team that we wanted to create an interactive globe that showed COVID-19 cases per country. However, we weren’t sure what we wanted our color gradient to represent. After careful review, we decided that communicating information based on the population was more transparent. Also, we added each country’s GDP ($ per capita) to our visualization as it can be a good indicator of the number of COVID-19 cases that each country can have.</p>
    <h2>Division of Labor</h2>
    <ul>
        <li>Diego proposed the interactive globe visualization idea and, together with Hector, obtained the dataset containing the countries' COVID-19 cases throughout 2020-2021.</li>
        <li>Andrew was responsible for developing the website and coding the interactive visualization, ensuring that the interactive techniques and tooltip were working smoothly.</li>
        <li>Diego and Hector were in charge of cleaning, aggregating, and manipulating the datasets and turning them into JSON format for use in D3.</li>
        <li>Hector wrote the write-up once everyone’s work had been assembled together.</li>
    </ul>
    <h2>Challenges</h2>
    <p>We faced multiple challenges, especially in data cleaning to ensure all the data had matching country names and integrating the globe visualization into Svelte. This project required extensive collaboration, data manipulation, and technical development to achieve our goal of creating an engaging and informative visualization.</p>

</div>
</main>