<script>
  // @ts-nocheck

  import Number from "svelte-digital-alarm-number";
  import { SvelteToast, toast } from "@zerodevx/svelte-toast";

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
      toast.push("Good job!", {
        theme: {
          "--toastColor": "mintcream",
          "--toastBackground": "rgba(72,187,120,0.9)",
          "--toastBarBackground": "#2F855A",
        },
      });
      if (level === 2) {
        toast.push(`<strong>You win, go for a beer</strong>😛`, {
          pausable: true,
        });
      }
    } else {
      retries = retries + 1;
      toast.push("Good try!", {
        theme: {
          "--toastColor": "mintcream",
          "--toastBackground": "#f44336",
          "--toastBarBackground": "#f44336",
        },
      });
    }
  }
</script>

<h2>Try to stop the clock at 10:00</h2>

<SvelteToast />

<Number min="2" value={num} />
<Number min="7" value={num2} />

{#if is_running}
  <button class="button button-stop" on:click={stop}>stop</button>
{:else}
  <button class="button button-start" on:click={start}>start</button>
{/if}

<br />
Difficulty:
<select
  bind:value={level}
  style="padding: 10px; background-color:rgb(217 118 16); border:none; color:black; font-size:1.5em; border-radius: 5px"
>
  <option value={1}>Noob</option>
  <option value={2}>Pro</option>
</select>

<h3>Attempts: {retries}</h3>

<style>
  .button {
    border: none;
    color: white;
    padding: 30px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin-top: 15px;
    margin-bottom: 15px;
    font-size: 2em;
    border-radius: 50%;
    width: 150px;
    height: 150px;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px,
      rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px,
      rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
  }

  .button-start {
    background-color: #4caf50;
  }

  .button-stop {
    background-color: #f44336;
  }
</style>
