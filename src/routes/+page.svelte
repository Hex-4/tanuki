<style>
  @font-face {
    font-family: FOT-Seurat Pro B;
    src: url("/fonts/FOT-Seurat Pro B.otf");
  }
</style>


<script lang="ts">
  import { load, Store } from "@tauri-apps/plugin-store";
  let hourlyAudio: HTMLAudioElement = $state();
  let rainAudio: HTMLAudioElement = $state();

  /// From https://github.com/mn6/nook-desktop/blob/main/app/hidden/player.js, thanks :D ///
  const games = [
    "random",
    "kk-slider-desktop",
    "population-growing",
    "population-growing-snowy",
    "population-growing-cherry",
    "wild-world",
    "wild-world-rainy",
    "wild-world-snowy",
    "new-leaf",
    "new-leaf-rainy",
    "new-leaf-snowy",
    "new-horizons",
    "new-horizons-rainy",
    "new-horizons-snowy",
    // removed Pocket Camp as it has morning/afternoon/evening/night tracks which im too lazy to implement
    
  ];

  const kkSongs = [
    "Agent K.K.",
    "Aloha K.K.",
    "Animal City",
    "Bubblegum K.K.",
    "Cafe K.K.",
    "Comrade K.K.",
    "DJ K.K.",
    "Drivin",
    "Farewell",
    "Forest Life",
    "Go K.K. Rider!",
    "Hypno K.K.",
    "I Love You",
    "Imperial K.K.",
    "K.K. Adventure",
    "K.K. Aria",
    "K.K. Ballad",
    "K.K. Bazaar",
    "K.K. Birthday",
    "K.K. Blues",
    "K.K. Bossa",
    "K.K. Calypso",
    "K.K. Casbah",
    "K.K. Chorale",
    "K.K. Condor",
    "K.K. Country",
    "K.K. Cruisin",
    "K.K. D&B",
    "K.K. Dirge",
    "K.K. Disco",
    "K.K. Dixie",
    "K.K. Faire",
    "K.K. Flamenco",
    "K.K. Folk",
    "K.K. Fusion",
    "K.K. Groove",
    "K.K. Gumbo",
    "K.K. House",
    "K.K. Island",
    "K.K. Jazz",
    "K.K. Jongara",
    "K.K. Lament",
    "K.K. Love Song",
    "K.K. Lullaby",
    "K.K. Mambo",
    "K.K. Marathon",
    "K.K. March",
    "K.K. Metal",
    "K.K. Milonga",
    "K.K. Moody",
    "K.K. Oasis",
    "K.K. Parade",
    "K.K. Ragtime",
    "K.K. Rally",
    "K.K. Reggae",
    "K.K. Rock",
    "K.K. Rockabilly",
    "K.K. Safari",
    "K.K. Salsa",
    "K.K. Samba",
    "K.K. Ska",
    "K.K. Sonata",
    "K.K. Song",
    "K.K. Soul",
    "K.K. Steppe",
    "K.K. Stroll",
    "K.K. Swing",
    "K.K. Synth",
    "K.K. Tango",
    "K.K. Technopop",
    "K.K. Waltz",
    "K.K. Western",
    "K.K. Etude",
    "King K.K.",
    "Lucky K.K.",
    "Marine Song 2001",
    "Mountain Song",
    "Mr. K.K.",
    "My Place",
    "Neapolitan",
    "Only Me",
    "Pondering",
    "Rockin K.K.",
    "Senor K.K.",
    "Soulful K.K.",
    "Space K.K.",
    "Spring Blossoms",
    "Stale Cupcakes",
    "Steep Hill",
    "Surfin K.K.",
    "The K. Funk",
    "To the Edge",
    "Two Days Ago",
    "Unknown 01",
    "Unknown 02",
    "Wandering",
    "Welcome Horizons",
    "Wild World",
    "Agent K.K. (Radio)",
    "Aloha K.K. (Radio)",
    "Animal City (Radio)",
    "Bubblegum K.K. (Radio)",
    "Cafe K.K. (Radio)",
    "Comrade K.K. (Radio)",
    "DJ K.K. (Radio)",
    "Drivin (Radio)",
    "Farewell (Radio)",
    "Forest Life (Radio)",
    "Go K.K. Rider! (Radio)",
    "Hypno K.K. (Radio)",
    "I Love You (Radio)",
    "Imperial K.K. (Radio)",
    "K.K. Adventure (Radio)",
    "K.K. Aria (Radio)",
    "K.K. Ballad (Radio)",
    "K.K. Bazaar (Radio)",
    "K.K. Birthday (Radio)",
    "K.K. Blues (Radio)",
    "K.K. Bossa (Radio)",
    "K.K. Calypso (Radio)",
    "K.K. Casbah (Radio)",
    "K.K. Chorale (Radio)",
    "K.K. Condor (Radio)",
    "K.K. Country (Radio)",
    "K.K. Cruisin (Radio)",
    "K.K. D&B (Radio)",
    "K.K. Dirge (Radio)",
    "K.K. Disco (Radio)",
    "K.K. Dixie (Radio)",
    "K.K. Faire (Radio)",
    "K.K. Flamenco (Radio)",
    "K.K. Folk (Radio)",
    "K.K. Fusion (Radio)",
    "K.K. Groove (Radio)",
    "K.K. Gumbo (Radio)",
    "K.K. House (Radio)",
    "K.K. Island (Radio)",
    "K.K. Jazz (Radio)",
    "K.K. Jongara (Radio)",
    "K.K. Lament (Radio)",
    "K.K. Love Song (Radio)",
    "K.K. Lullaby (Radio)",
    "K.K. Mambo (Radio)",
    "K.K. Marathon (Radio)",
    "K.K. March (Radio)",
    "K.K. Metal (Radio)",
    "K.K. Milonga (Radio)",
    "K.K. Moody (Radio)",
    "K.K. Oasis (Radio)",
    "K.K. Parade (Radio)",
    "K.K. Ragtime (Radio)",
    "K.K. Rally (Radio)",
    "K.K. Reggae (Radio)",
    "K.K. Rock (Radio)",
    "K.K. Rockabilly (Radio)",
    "K.K. Safari (Radio)",
    "K.K. Salsa (Radio)",
    "K.K. Samba (Radio)",
    "K.K. Ska (Radio)",
    "K.K. Sonata (Radio)",
    "K.K. Song (Radio)",
    "K.K. Soul (Radio)",
    "K.K. Steppe (Radio)",
    "K.K. Stroll (Radio)",
    "K.K. Swing (Radio)",
    "K.K. Synth (Radio)",
    "K.K. Tango (Radio)",
    "K.K. Technopop (Radio)",
    "K.K. Waltz (Radio)",
    "K.K. Western (Radio)",
    "K.K. Etude (Radio)",
    "King K.K. (Radio)",
    "Lucky K.K. (Radio)",
    "Marine Song 2001 (Radio)",
    "Mountain Song (Radio)",
    "Mr. K.K. (Radio)",
    "My Place (Radio)",
    "Neapolitan (Radio)",
    "Only Me (Radio)",
    "Pondering (Radio)",
    "Rockin K.K. (Radio)",
    "Senor K.K. (Radio)",
    "Soulful K.K. (Radio)",
    "Space K.K. (Radio)",
    "Spring Blossoms (Radio)",
    "Stale Cupcakes (Radio)",
    "Steep Hill (Radio)",
    "Surfin K.K. (Radio)",
    "The K. Funk (Radio)",
    "To the Edge (Radio)",
    "Two Days Ago (Radio)",
    "Wandering (Radio)",
    "Welcome Horizons (Radio)",
  ];
  /// End stolen code ///
  const humanGames = [
    "Random",
    "K.K. Slider",
    "Population Growing",
    "Population Growing (Snowy)",
    "Population Growing (Cherry Blossom)",
    "Wild World",
    "Wild World (Rainy)",
    "Wild World (Snowy)",
    "New Leaf",
    "New Leaf (Rainy)",
    "New Leaf (Snowy)",
    "New Horizons",
    "New Horizons (Rainy)",
    "New Horizons (Snowy)",
    
  ];

  import { onMount } from "svelte";

  let musicURL = $derived.by(() => {
    if (selectedGame === 1) { // K.K Slider
      return `https://d17orwheorv96d.cloudfront.net/kk-slider-desktop/${kkSongs[currentHour]}.ogg`;
    } else if (selectedGame === 0) { // Random
      const randomGameIndex = Math.floor(Math.random() * (games.length - 1)) + 1;
      return `https://d17orwheorv96d.cloudfront.net/${games[randomGameIndex]}/${hours[currentHour]}.ogg`;
    } else {
      return `https://d17orwheorv96d.cloudfront.net/${games[selectedGame]}/${hours[currentHour]}.ogg`;
    }
    
  });

  let store: Store;

  let selectedGame = $state(6); // New Leaf
  let currentHour = $state(20); // 8 PM

  const hours = [
    "12am",
    "1am",
    "2am",
    "3am",
    "4am",
    "5am",
    "6am",
    "7am",
    "8am",
    "9am",
    "10am",
    "11am",
    "12pm",
    "1pm",
    "2pm",
    "3pm",
    "4pm",
    "5pm",
    "6pm",
    "7pm",
    "8pm",
    "9pm",
    "10pm",
    "11pm",
  ];
  const humanHours = [
    "12 AM",
    "1 AM",
    "2 AM",
    "3 AM",
    "4 AM",
    "5 AM",
    "6 AM",
    "7 AM",
    "8 AM",
    "9 AM",
    "10 AM",
    "11 AM",
    "12 PM",
    "1 PM",
    "2 PM",
    "3 PM",
    "4 PM",
    "5 PM",
    "6 PM",
    "7 PM",
    "8 PM",
    "9 PM",
    "10 PM",
    "11 PM",
  ];

  let isHourlyPlaying = $derived(true);
  let isRainPlaying = $derived(true);
  let currentKKSong = $derived(0);

  function updateHour() {
    const date = new Date();
    currentHour = date.getHours();

    if (selectedGame === 1) {
      currentHour = currentKKSong
      currentKKSong++
      if (currentKKSong >= kkSongs.length) {
        currentKKSong = 0; 
      }
    }
  }

  onMount(async () => {
    store = await load("storeewew.json", { autoSave: 100 });
    const val = await store.get<{ value: number }>("selectedGame");
    if (val && val.value !== undefined) {
      selectedGame = val.value;
    }
    updateHour();
    hourlyAudio.load();
  });
  let time = $state(new Date());
  let timeString = $derived(
    time.toLocaleTimeString("en-US", { hour: "2-digit", minute: "2-digit" })
  );

  $effect(async () => {

    updateHour()

    console.log("selected game changed:", selectedGame);
    if (store && selectedGame !== undefined) {
      console.log("saving selected game:", selectedGame);
      await store.set("selectedGame", { value: selectedGame });
    }
  });

  setInterval(() => {
    time = new Date();
  }, 30000);

  function hourlyEnded() {
    hourlyAudio.currentTime = 0;
    hourlyAudio.play();
    updateHour();
  }

  function rainEnded() {
    rainAudio.currentTime = 0;
    rainAudio.play();
  }
</script>

<div class="w-full h-full flex flex-col items-center justify-center">
  <audio
    bind:this={hourlyAudio}
    src={musicURL}
    autoplay
    onended={hourlyEnded}
  />
  <audio
    bind:this={rainAudio}
    src="https://d17orwheorv96d.cloudfront.net/rain/game-rain.ogg"
    autoplay
    onended={rainEnded}
  />
  <p class=" text-stone-500">{timeString}</p>
  <p class="italic">{isHourlyPlaying ? "now playing:" : "current track:"}</p>

  <p class="text-2xl my-2 mx-4 justify-center text-center">
    {humanHours[currentHour]} <span class="text-stone-500">·</span>
    {humanGames[selectedGame]}
  </p>

  <div class="flex flex-row items-center justify-center mt-2">
    <button
      class="transition mx-4 {isHourlyPlaying
        ? 'scale-100'
        : 'scale-85 greyscale'} hover:scale-110"
      aria-label="Play/Pause music"
      title="Play/Pause music"
      onclick={async () => {
        if (hourlyAudio.paused) {
          updateHour();
          await hourlyAudio.play();
          isHourlyPlaying = true;
        } else {
          hourlyAudio.pause();
          isHourlyPlaying = false;
        }
      }}
    >
      <img
        src="/icons/music.png"
        class="h-10 {isHourlyPlaying
          ? ''
          : 'grayscale'} inline transition-all transition-discrete"
        alt=""
      />
    </button>
    {#if hourlyAudio}
      <input
        type="range"
        min="0.3"
        max="1"
        step="0.01"
        bind:value={hourlyAudio.volume}
        class="h-8 hourly-slider"
      />
    {/if}
  </div>

  <div class="flex flex-row items-center justify-center mt-4">
    <button
      class="transition mx-4 {isRainPlaying
        ? 'scale-100'
        : 'scale-85 greyscale'} hover:scale-110"
      aria-label="Play/Pause rain"
      title="Play/Pause music"
      onclick={async () => {
        if (rainAudio.paused) {
          await rainAudio.play();
          isRainPlaying = true;
        } else {
          rainAudio.pause();
          isRainPlaying = false;
        }
      }}
    >
      <img
        src="/icons/snowflake.png"
        class="h-10 {isRainPlaying
          ? ''
          : 'grayscale'} inline transition-all transition-discrete"
        alt=""
      />
    </button>
    {#if rainAudio}
      <input
        type="range"
        min="0.1"
        max="0.8"
        step="0.01"
        bind:value={rainAudio.volume}
        class="h-8 hourly-slider"
      />
    {/if}
  </div>
  <div class="relative inline-block mt-6 w-6/8 text-sm">
    <select
      bind:value={selectedGame}
      class="group appearance-none bg-orange-200 p-1.5 pl-5 rounded-lg w-full hover:bg-orange-300 transition cursor-pointer"
      aria-label="Select game"
    >
      {#each humanGames as game, index}
        <option value={index}>{game}</option>
      {/each}
    </select>

    <!-- custom arrow -->
    <div
      class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-3 text-stone-950"
    >
      <img src="/icons/down-caret.svg" class="size-8" alt="Dropdown arrow" />
    </div>
  </div>
</div>

