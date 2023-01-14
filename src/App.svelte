<script>
  import Dice from './lib/Dice.svelte'

  const positions = [
    ["20%", "50%"],
    ["15%", "35%"],
    ["25%", "15%"],
    ["75%", "35%"],
    ["45%", "37%"],
    ["74%", "65%"],
    ["54%", "25%"],
    ["34%", "55%"],
    ["52%", "49%"],
    ["49%", "62%"],
    ["29%", "73%"],
    ["79%", "45%"],
    ["29%", "31%"],
    ["54%", "72%"],
  ]

  const diceNumbers = []
  const dicePositions = []

  for (let i = 0; i < 5; i++) {
    diceNumbers.push(randInt(1, 6));
    dicePositions.push(getPosition());
  }

  function getPosition() {
    let newPos = positions[randInt(0, positions.length)];
    if (dicePositions.includes(newPos)) {
      return getPosition();
    }
    return newPos;
  }

  function randInt(start, end) {
    return Math.floor(Math.random()*end+start);
  }
</script>

<main>
  <div class="dice-box">
    {#each diceNumbers as dice, i}
      <Dice number={dice} x={dicePositions[i][0]} y={dicePositions[i][1]}/>
    {/each}
  </div>
</main>

<style>
  .dice-box {
    height: 800px;
    width: 800px;
    background-color: burlywood;
    border: 10px solid brown;
    border-radius: 50%;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
</style>
