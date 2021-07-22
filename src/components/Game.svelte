<script>
  import Commands from './Commands.svelte'
  import Card from './Card.svelte'

  let numberOfCards = 3
  let openSpeed = 0.4
  let openTime = 1
  let type = 'number'

  let numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29]
  let letters = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']

  let cards
  let selection
  let working
  let nbOpenedCard
  let asyncWorkId

  function initGame() {
    working = true
    if (asyncWorkId) clearTimeout(asyncWorkId)
    selection = undefined
    nbOpenedCard = 0
      let cardsValue = shuffle(type === 'number' ? numbers : letters)
    cards = Array.from(Array(numberOfCards).keys()).reduce((acc, index) => {
      let value = cardsValue[index]
      acc.push({ value, state: 0 })
      acc.push({ value, state: 0 })
      return acc;
    }, [])
    cards = shuffle(cards)
    working = false
  }

  function shuffle(array) {
    return array.map(v => ({r: Math.random(), v}))
                .sort((a, b) => a.r - b.r)
                .map(v => v.v)
  }

  function click(index) {
    if (working) {
      return
    }
    nbOpenedCard ++
    if (selection === undefined) {
      selection = index
      cards[index].state = 1
    } else if (cards[selection].value === cards[index].value) {
      cards[selection].oldState = 1
      cards[selection].state = 2
      cards[index].state = 2
      selection = undefined
    } else {
      working = true
      cards[index].state = 1
      asyncWorkId = setTimeout(() => {
        cards[selection].state = 0
        cards[index].state = 0
        selection = undefined
        working = false
      }, openTime * 1000)
    }
  }

  initGame()
</script>

<style>
  main {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
  }
  div {
    text-align: center;
  }
  @media (min-width: 700px) {
    main {
      margin: 10%;
      width: 80%
    }
  }
</style>
<Commands newGame={initGame} bind:numberOfCards bind:openSpeed bind:openTime bind:type/>
<main>
  {#each cards as card, index (`${index}-${card.value}`)}
    <Card
      {openSpeed}
      value={card.value}
      state={card.state}
      oldState={card.oldState}
      on:click={() => click(index)}
    />
  {/each}
</main>
<div>
  Nomber of guesses : {nbOpenedCard} !
</div>