<script>
  import Dice from './lib/Dice.svelte'

  const positions = [
    ["48%", "47%"],
    ["66%", "48%"],
    ["33%", "35%"],
    ["59%", "32%"],
    ["30%", "55%"],
    ["47%", "62%"],
    // ["20%", "25%"],
    // ["25%", "55%"],
  ]

  let diceAmount = 5;
  let diceNumbers = [];
  let dicePositions = [];
  let rotate = [];

  let logStack = [];
  let logVisible = false;

  function rollDice() {
    logAction(`Dice Roll (${diceAmount} dice)`);
    diceNumbers = [];
    dicePositions = [];
    rotate = [];
    for (let i = 0; i < diceAmount; i++) {
      diceNumbers.push(randInt(1, 6));
      dicePositions.push(getPosition());
      rotate.push(randInt(0, 25) + "deg");
    }
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

  function removeDice() {
    if (diceAmount > 0) {
      logAction(`Dice Removed, ${diceAmount} dice left`);
      diceNumbers.pop();
      dicePositions.pop();
      rotate.pop();
      diceNumbers = [...diceNumbers]
      dicePositions = [...dicePositions]
      rotate = [...rotate]
      diceAmount--;
    }
  }

  function resetDice() {
    logAction(`Dice Reset.`);
    if (diceAmount < 5) {
      diceAmount = 5;
      rollDice();
    }
  }

  function logAction(action) {
    let timeNow = new Date();
    logStack.push(`[${timeNow.toLocaleTimeString('en-US')}] - ${action}`);
    console.log(logStack);
  }

  logAction(`Game started.`);
  rollDice();
</script>

<main class="main">
  <div class="buttons">
    <button style="background-color: orange; color: black;" on:click={removeDice}>Remove</button>
    <button style="background-color: green; color: white;" on:click={rollDice} id="reroll">Roll</button>
    <button style="background-color: red; color: white;" on:click={resetDice}>Reset</button>
    <button on:click={() => {logVisible = true}}>Log</button>
  </div>
  <div class="dice-box">
    <!-- {#each positions as dice}
    <Dice number={2} x={dice[0]} y={dice[1]}/>
    {/each} -->
    {#each diceNumbers as dice, i}
      <Dice number={dice} x={dicePositions[i][0]} y={dicePositions[i][1]} --rotate={rotate[i]}/>
    {/each}
  </div>
  {#if logVisible}
  <div class="log">
    <div class="buttons" style="margin-bottom: 10px;">
      <button style="position: fixed; top: 10%;" on:click={() => {logVisible = false}}>Hide Log</button>
    </div>
    {#each logStack as logItem, i}
      <div>{logItem}</div>
    {/each}
  </div>
  {/if}
</main>

<style>

  .main {
    display: flex;
    width: 100%;
    /* height: 100%; */
    gap: 20px;
    flex-direction: column;
    justify-content: center;
    align-items: center;

  }

  button {
    padding: 10px;
    min-width: 100px;
    background-color: blanchedalmond;
    font-size: 18px;
    border-color: grey;
  }

  .buttons {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    /* position: absolute; */
    /* top: 8%; */
    /* left: 50%; */
    /* transform: translate(-50%, 0%); */
    color: white;
    width: 70%;
    justify-content: center;
    align-items: center;
  }
  .dice-box {
    height: 50rem;
    width: 50rem;
    background-color: purple;
    border: 10px solid lightblue;
    border-radius: 50%;
    position: relative;
    /* left: 50%; */
    /* top: 55%; */
    /* transform: translate(-50%, -50%); */
  }

  .log {
    position: absolute;
    /* left: 50%; */
    /* right: 50%; */
    /* transform: translate(-50%, -50%); */
    background-color: black;
    height: 100%;
    width: 100%;
    color: white;
    display: flex;
    box-sizing: border-box;
    padding: 10%;
    flex-direction: column;
    overflow-y: scroll;
    justify-content: center;
    align-items: center;
    z-index: 2;
  }

  @media screen and (max-width: 900px) {
    .dice-box {
      height: 380px;
      width: 380px;
      }
    }
</style>
