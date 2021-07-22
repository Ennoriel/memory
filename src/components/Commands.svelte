<script>
  export let newGame
  export let numberOfCards
  export let openSpeed
  export let openTime
  export let type
</script>

<style>
  span {
    width: 100%;
    margin: 10px 0;
    height: 80px;
    border-top: 1px solid #444;
    border-bottom: 1px solid #444;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    flex-wrap: wrap;
  }
  div {
    position: relative;
    margin-bottom: 20px;
    min-width: 165px;
    text-align: center;
  }
  input[type="range"] {
    position: absolute;
    left: 50%;
    top: 20px;
    transform: translateX(-50%);
    margin: 0;
  }
  button {
    padding: 0.3em;
    font-size: 16px;
  }
  .radio-wrapper {
    margin: 0;
  }
</style>

<span>
  <div>
    <label for="open-speed">
      Card openning duration ({openSpeed}s)
    </label>
    <input type="range" name="open-speed" min=0 max=1 step=0.1 bind:value={openSpeed}/>
  </div>

  <div>
    <label for="open-time">
      Card display time ({openTime}s)
    </label>
    <input type="range" name="open-time" min=0.5 max=5 step=0.1 bind:value={openTime}/>
  </div>

  <div>
    <label for="number-of-cards">
      Number of cards ({2 * numberOfCards})
    </label>
    <input type="range" name="number-of-cards" min=3 max=20 bind:value={numberOfCards}/>
  </div>

  <div class="radio-wrapper">
    <label for="type-number">
      Numbers
    </label>
    <input type="radio" name="type-number" value="number" bind:group={type}/>
    <input type="radio" name="type-letter" value="type-letter" bind:group={type}/>
    <label for="type-letter">
      Letters
    </label>
  </div>

  <button on:click={newGame}>New Game</button>
</span>