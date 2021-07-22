<script>

  export let value
  export let state
  export let oldState
  export let openSpeed

  function spin() {
    return {
      delay: 0,
      duration: openSpeed * 1000,
      css: (t, u) => {
        return `transform: rotateY(${180 * t}deg); color: ${t > 0.5 ? "white" : "black"}; background-color: ${t > 0.5 ? "white" : "#eee"};`
      }
    };
  }

  function delay() {
    return {
      delay : oldState ? 0 : openSpeed * 1000,
      duration: 0,
      css: (t, u) => {
        return `opacity: 0;`
      }
    };
  }
</script>

<style>
  .card-wrapper {
    position: relative;
    width: 50px;
    height: 50px;
    margin: 1em;
  }
  .card {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    line-height: 50px;
    text-align: center;
    box-shadow: 0 0 5px 0 gray;
    cursor: pointer;

    font-size: 30px;
    font-weight: bold;
  }
  .card:not(.card-selected):not(.card-validated):hover {
    box-shadow: 0 0 10px 2px darkslategray;
  }
  .card-selected {
    background-color: #eee;
  }
  .card-validated {
    background-color: #aea;
    cursor: default;
  }
  .card-unselected {
    color: white
  }
</style>

<div class="card-wrapper">
  {#if state === 0}
    <div class="card card-unselected" on:click transition:spin|local>
      {value}
    </div>
  {:else if state === 2}
    <div class="card card-validated" transition:delay|local>
      {value}
    </div>
  {:else if state === 1}
    <div class="card card-selected" on:click transition:delay|local>
      {value}
    </div>
  {/if}
</div>