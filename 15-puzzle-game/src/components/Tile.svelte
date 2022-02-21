<script>
    export let value = -1;
    export let num_tiles = -1;
    let font_size = 0

    const computeFontSize = (_nt) => {
        font_size = (1/Math.sqrt(_nt)) * 10
    }

    $: {
        computeFontSize(num_tiles)
    }

</script>

<!--html-->
<button on:click|preventDefault>
    <div class="outer">
        {#if value != 0}
            <tile style={`font-size: ${font_size}em`}>
                {value}
            </tile>
        {:else}
            <tile class='empty' style={`font-size: ${font_size}em`}>0</tile> <!--&nbsp;-->
        {/if}
    </div>
</button>

<!--end-html-->

<!--local-styles-->
<style>
button {
    background: none;
    border: none;
    cursor: pointer;
}

button:hover {
    background: none;
    color: none;
}
button:focus {
    background: none;
    color:  none;
}

.empty {
    opacity: 0;
}

.outer {

    max-width: 10rem;
    max-height: 10rem;
    width: 100%;
    height: 100%;
    border: solid aliceblue;
    background-color: palevioletred;
    border-radius: 5px;
    display: flex;
    align-items: center;
    vertical-align: middle;

    justify-content: center;
}

tile {
    /* font size in JSX */
    font-weight: bold;
    color: aliceblue;
    display: inline;
    font-family: 'Trebuchet MS', 'Arial', sans-serif;
}


</style>