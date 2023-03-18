<script>
    export let number;
    const map = ["one", "two", "three", "four", "five", "six"]
    export let x;
    export let y;
</script>

<main>
    <div class="dice" style:left={x} style:top={y}>
        <img class="dice-img" src="/{map[number-1]}.png" alt="dice" />
    </div>
</main>

<style>
    .dice {
        position: absolute;
        transform: rotate(var(--rotate));
    }

    .dice-img {
        height: 70px;
        width: 70px;
    }

    @media screen and (max-width: 900px) {
    .dice-img {
      height: 50px;
      width: 50px;
      }
    }

</style>