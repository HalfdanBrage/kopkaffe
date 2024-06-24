<script lang="ts">
  let dimensions = [10, 10]

  function generateEmptyGrid(x: number, y: number) {
    let out:number[][] = []
    for (let i = 0; i < x; i++) {
      out[i]=[]
      for (let j = 0; j < y; j++) {
        out[i][j] = 0
      }
    }
    return out
  }

  let grid:number[][] = generateEmptyGrid(dimensions[0], dimensions[1])

  function swap(x: number, y: number) {
    grid[x][y] = grid[x][y] == 0 ? 1 : 0
    grid = grid
  }
</script>

<svelte:head>
	<title>About</title>
	<meta name="description" content="About this app" />
</svelte:head>

<div class="text-column">
	<h1>Cellular Automata Game!</h1>

  <div class="grid">
    {#each grid as column, i}
      <div class="column">
        {#each column as val, j}
          <button 
            on:click={() => swap(i, j)}
            class="cell-{val}"
          />
        {/each}
      </div>
    {/each}
  </div>
</div>

<style>

  .grid {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
  }
  .column {
    display: flex;
    justify-content: space-evenly;
  }

  .cell-0, .cell-1, .cell-0:focus, .cell-1:focus{
    flex-grow: 1;
    align-self: auto;
    aspect-ratio: 1 / 1;
    border-style: none;
    outline: 1px solid black;
  }

  .cell-0 {
    background-color: white;
  }

  .cell-1 {
    background-color: black;
  }
</style>
