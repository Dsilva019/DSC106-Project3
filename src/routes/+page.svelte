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
</style>

<div class="legend">
    <div class="data-not-available"></div><span>Data Not Available</span>
    <div class="low"></div><span>Low Cases per Population</span>
    <div class="medium"></div><span>Medium Cases per Population</span>
    <div class="high"></div><span>High Cases per Population</span>
    <div class="very-high"></div><span>Very High Cases per Population</span>
</div>
</main>