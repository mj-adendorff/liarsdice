<script>
    export let number;
    const map = ["one", "two", "three", "four", "five", "six"]
    export let x;
    export let y;
</script>

<main>
    <div class="dice" style:left={x} style:top={y}>
        <img src="/{map[number-1]}.png" alt="dice" height="70px" width="70px"/>
    </div>
</main>

<style>
    .dice {
        position: absolute;
    }
</style>