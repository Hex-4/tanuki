<script lang="ts">
  import Visualizer from "../lib/visualizer.svelte";
  let hourlyAudio: HTMLAudioElement = $state();

  import { onMount } from "svelte";

  onMount(() => {
    hourlyAudio.src = "/music/new-leaf/7pm.mp3";

    hourlyAudio.load();
  });
  let time = $state(new Date());
  let timeString = $derived(
    time.toLocaleTimeString("en-US", { hour: "2-digit", minute: "2-digit" })
  );

  setInterval(() => {
    time = new Date();
  }, 1000);

  function hourlyEnded() {
    hourlyAudio.currentTime = 0;
    hourlyAudio.play();
  }
</script>

<div class="w-full h-full flex flex-col items-center justify-center">
  <audio bind:this={hourlyAudio} autoplay onended={hourlyEnded} />
  <p class=" text-stone-500">{timeString}</p>
  <p class="italic">now playing:</p>

  <p class="text-3xl my-2">
    7 PM <span class="text-stone-500">·</span> New Leaf
  </p>

  {#if hourlyAudio}
    <Visualizer audioElement={hourlyAudio}></Visualizer>
  {/if}

  <button
    class="p-2 outline rounded-full px-3 my-4"
    onclick={async () => {
      if (hourlyAudio.paused) {
        await hourlyAudio.play();
      } else {
        hourlyAudio.pause();
      }
    }}
  >
    >
  </button>
</div>

<style>
</style>
