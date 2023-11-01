<script>
// @ts-nocheck

  import Number from "svelte-digital-alarm-number";

  let level = 1;
  let is_running = false;
  let retries = 0;
  let num = 0;
  let num2 = 0;
  let inter;

  let levels = {
    1: 100,
    2: 10,
  };
  function pad(num, size) {
    num = num.toString();
    while (num.length < size) num = "0" + num;
    return num;
  }

  function reset() {
    clearInterval(inter);
    num = 0;
    num2 = 0;
  }

  function start() {
    reset();
    is_running = true;
    let levelw = levels[level];
    inter = setInterval(() => {
      num2 = num2 + 1;
      if (num2 === 10) {
        num = num + 1;
        num2 = 0;
      }
    }, levels[level]);
  }

  function stop() {
    is_running = false;
    clearInterval(inter);

    if (num === 10 && num2 === 0) {
      console.log("gano");
    } else {
      retries = retries + 1;
      console.log("intente de nuevo");
    }
  }
</script>

<div>Try to stop the clock at 10:00</div>

<Number min="2" value={num} />
<Number min="7" value={num2} />

{#if is_running}
  <button on:click={stop}>stop</button>
{:else}
  <button on:click={start}>start</button>
{/if}

<select bind:value={level}>
  <option value={1}>easy</option>
  <option value={2}>hard</option>
</select>

<div>Attempts: {retries}</div>
