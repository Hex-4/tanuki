<script lang="ts">
  // @refresh reload
  import "../app.css";
  import { onMount } from "svelte";
  let { audioElement } = $props();

  let audioContext: AudioContext | null = $state(null);
  let analyser: AnalyserNode | null = $state(null);
  let dataArray: Uint8Array | null = $state(null);
  let source: MediaElementAudioSourceNode = $state()
  let volume = $state(0);

  export function setupVisualizer() {
    audioContext = new AudioContext();
    analyser = audioContext.createAnalyser();
    analyser.fftSize = 64;
    dataArray = new Uint8Array(analyser.frequencyBinCount);

    source = audioContext.createMediaElementSource(audioElement);
    source.connect(analyser);
    analyser.connect(audioContext.destination);
  }

  function updateVolume() {
    if (!analyser) return;
    volume = getAverageVolume();
    requestAnimationFrame(updateVolume);
  }

  onMount(() => {
    setupVisualizer();
    updateVolume();

    return () => {
      if (analyser) analyser.disconnect();
      if (audioContext) audioContext.close();
      if (source) source.disconnect();
    };
  });

  function getAverageVolume() {
    if (!analyser || !dataArray) return 0;

    analyser.getByteFrequencyData(dataArray);
    const average =
      dataArray.reduce((sum, val) => sum + val, 0) / dataArray.length;
    return average / 255; 
  }
</script>

<div class="bg-stone-800 h-2 rounded-3xl my-1" style="width: {Math.min(volume * 200, 100)}%"></div>
