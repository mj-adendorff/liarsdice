<script>
  import Dice from './lib/Dice.svelte'

  const positions = [
    ["48%", "47%"],
    ["58%", "45%"],
    ["33%", "35%"],
    ["59%", "34%"],
    ["38%", "55%"],
    ["45%", "37%"],
    ["47%", "27%"],
    ["35%", "45%"],
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

<main>
  <div class="buttons">
    <button on:click={removeDice}>Remove Dice</button>
    <button on:click={rollDice} id="reroll">Roll Again</button>
    <button on:click={resetDice}>Reset Dice</button>
    <button on:click={() => {logVisible = true}}>Show Log</button>
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
    <div class="buttons">
      <button on:click={() => {logVisible = false}}>Hide Log</button>
    </div>
    {#each logStack as logItem, i}
      <div>{logItem}</div>
    {/each}
  </div>
  {/if}
</main>

<style>

  button {
    padding: 10px;
    min-width: 100px;;
    background-color: blanchedalmond;
    font-size: 16px;
  }

  .buttons {
    display: flex;
    gap: 10px;
    position: absolute;
    top: 10%;
    left: 50%;
    transform: translate(-50%, 0%);
    color: white;
  }
  .dice-box {
    height: 50rem;
    width: 50rem;
    background-color: brown;
    border: 10px solid grey;
    border-radius: 50%;
    position: absolute;
    left: 50%;
    top: 55%;
    transform: translate(-50%, -50%);
  }

  .log {
    position: absolute;
    left: 50%;
    right: 50%;
    transform: translate(-50%, -50%);
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
  }

  @media screen and (max-width: 900px) {
    .dice-box {
      height: 400px;
      width: 400px;
      }
    }
</style>
