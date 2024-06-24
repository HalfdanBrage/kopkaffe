<script lang="ts">
  let dimensions = [20, 20]
  let playing = false

  async function sleep(ms: number): Promise<void> {
      return new Promise((resolve) => setTimeout(resolve, ms));
  }

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

  let neighborRules = [
    [0, 0, 0, 1, 0, 0, 0, 0, 0],
    [0, 0, 1, 1, 0, 0, 0, 0, 0]
  ]

  function getCell(x: number, y: number) {
    if (x >= 0 && y >= 0 && x < dimensions[0] && y < dimensions[0]) {
      return grid[x][y] 
    } else return 0
  }

  function countNeighbors(x: number, y: number) {
    return (
      getCell(x+1, y) + 
      getCell(x+1, y+1) + 
      getCell(x+1, y-1) + 
      getCell(x, y+1) + 
      getCell(x, y-1) + 
      getCell(x-1, y) + 
      getCell(x-1, y+1) + 
      getCell(x-1, y-1)
    )
  }

  function updateGrid() {
    let newGrid:number[][] = generateEmptyGrid(dimensions[0], dimensions[1])
    for (let i = 0; i < newGrid.length; i++) {
      for (let j = 0; j < newGrid[i].length; j++) {
        newGrid[i][j] = neighborRules[grid[i][j]][countNeighbors(i, j)]
      }
    }
    console.log(newGrid)
    grid = newGrid
  }

  function swap(x: number, y: number) {
    grid[x][y] = grid[x][y] == 0 ? 1 : 0
    grid = grid
  }

  function toggle() {
    playing = !playing
    play()
  }

  async function play() {
    if (!playing) return
    
    await sleep(100)
    updateGrid()
    play()
  }
</script>

<svelte:head>
	<title>Cellgame</title>
	<meta name="description" content="About this app" />
</svelte:head>

<div class="text-column">
	<h1>Conways game of life!</h1>

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

  <div class="button-row">
    <button on:click={() => (grid = generateEmptyGrid(dimensions[0], dimensions[1]))}>
      clear
    </button>
    <button on:click={updateGrid}>
      step
    </button>
    <button on:click={toggle}>
      {playing ? "pause" : "play"}
    </button>
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

  .button-row {
    display: flex;
    justify-content: center;
  }

  .cell-0 {
    background-color: white;
  }

  .cell-1 {
    background-color: black;
  }
</style>
