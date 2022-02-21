<script>
    import Tile from "./Tile.svelte"
    export let size = -1;
    let win = false
    let dim = Math.sqrt(size)
    // get tiles
    let tiles = Array.from(Array(size).keys())
    tiles = tiles.sort((a, b) => 0.5 - Math.random()) // shuffle array

    const checkWin = () => {
        for (let i = 0; i < size - 1; i++) {
            var lm = tiles[i] == 0 ? 9 : tiles[i]
            var rm = tiles[i+1] == 0 ? 9 : tiles[i+1]
            if (lm > rm) return false;
        }
        return true;
    }
    
    const handleWin = () => {
        win = true
    }
    
    const isLegalMove = (ni, ei) => {
        var top = tiles[ni-dim]
        var bot = tiles[ni+dim]
        var left = (ni % dim == 0) ? null : tiles[ni-1]
        var right = (ni % dim == dim-1) ? null : tiles[ni+1]
        
        console.log([top, bot, left, right], [ni, ei])
        if ([top, bot, left, right].includes(tiles[ei])) {
            return true
        } 
        return false
    }

    const handleClick = (n) => {
        var empty_index = tiles.findIndex(ele => ele == 0);
        var n_index = tiles.findIndex(ele => ele == n);
        if (!isLegalMove(n_index, empty_index)) return
        var _tmp = tiles[n_index]
        tiles[n_index] = tiles[empty_index]
        tiles[empty_index] = _tmp
        if (checkWin()) {
            handleWin()
        }
    }

    const randomize = (steps) => {
        for (var i = 0; i < steps; i+=1) {
            var rni = Math.floor(Math.random() * size)
            var eni = Math.floor(Math.random() * size)
            var r = Math.random()
            if (isLegalMove(rni, eni) && r > .5) {
                var _tmp = tiles[rni]
                tiles[rni] = tiles[eni]
                tiles[eni] = _tmp
            }
        }
        win = false
    }

    const newArray = (_s) => {
        tiles = Array.from(Array(_s).keys()) 
        dim = Math.sqrt(tiles.length)
        randomize(10000)
        console.log(tiles)
    }

    $: {
        newArray(size)
    }

</script>


<!--html-->
<div class='grid-container' style={`grid-template-columns: repeat(${dim}, 1fr) `}>
    {#each tiles as tile}
        <Tile value={tile} on:click={() => handleClick(tile)} num_tiles={size}/>
    {/each}
    {#if win == true}
            <h1>win!</h1>
    {/if}
</div>
<!--end-html-->

<!--local-styles-->
<style>
.grid-container {
    width: 25vw;
    height: 25vw;
    display: grid;
    /* border: 2px solid red; */
    /* border-radius: 3rem; */
    padding: 2rem;
    gap: 0px;
}


</style>