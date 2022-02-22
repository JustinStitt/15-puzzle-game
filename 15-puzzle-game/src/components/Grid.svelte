<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    import { slide } from 'svelte/transition';

    import Tile from "./Tile.svelte"
    export let size = -1;
    let dim = Math.sqrt(size)
    // get tiles
    let tiles = []
    let clicks = 0;
    let win = false;

    const checkWin = () => {
        for (let i = 0; i < size - 1; i++) {
            var lm = tiles[i] == 0 ? size : tiles[i]
            var rm = tiles[i+1] == 0 ? size : tiles[i+1]
            if (lm > rm) return false;
        }
        return true;
    }
    
    const inc = () => {
        clicks += 1
    }

    const handleWin = (_win) => {
        win = _win
        dispatch('win', {
            win: _win,
        });
    }
    
    const isLegalMove = (ni, ei) => {
        if (ni < 0 || ni > size - 1) return false;
        var top = tiles[ni-dim]
        var bot = tiles[ni+dim]
        var left = (ni % dim == 0) ? null : tiles[ni-1]
        var right = (ni % dim == dim-1) ? null : tiles[ni+1]
        
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
            handleWin(true)
        }
        inc()
    }

    const randomize = (steps) => {
        for (var i = 0; i < steps; i+=1) {
            var eni = tiles.findIndex(ele => ele == 0);
            var legal_moves = []
            for (var j = eni - dim - 1; j < eni + dim + 1; j += 1) {
                if (!isLegalMove(j, eni) || j == eni) continue
                legal_moves.push(j)
            }
            var r = Math.floor(Math.random() * legal_moves.length)

            var _tmp = tiles[legal_moves[r]]
            tiles[legal_moves[r]] = tiles[eni]
            tiles[eni] = _tmp
        }
        handleWin(false)
        clicks = 0
    }

    const newArray = (_s) => {
        tiles = Array.from(Array(_s).keys()) 
        dim = Math.sqrt(tiles.length)
        randomize(100 * size)
        console.log(tiles)
    }

    $: {
        newArray(size)
    }

</script>


<!--html-->
<div class='grid-container' style={`grid-template-columns: repeat(${dim}, 1fr) `}>
    {#each tiles as tile}
        <span class:explode-on-win={win} transition:slide>
            <Tile value={tile} on:click={() => handleClick(tile)} num_tiles={size}/>
        </span>
    {/each}
    
</div>
<div>
    <h2>Count: {clicks}</h2>
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
    padding: 1rem;
    gap: 0px;
}

span {
    display: grid;
}

.explode-on-win {
    animation: _explode_ .5s;
}

@keyframes _explode_ {
    0% {
        transform: scale(1)  translate(0, 0);
        
	}
	50% {
        transform: scale(1.5) rotate(-45deg);

	}
	100% {
        transform: scale(1) translate(0, 0);
	}
}


</style>