<script lang="ts">
  import "../app.css";

  let { children } = $props();

  import { TrayIcon, type TrayIconOptions } from "@tauri-apps/api/tray";
  import { defaultWindowIcon } from "@tauri-apps/api/app";
  import { Menu } from "@tauri-apps/api/menu";
  import { getCurrentWindow } from "@tauri-apps/api/window";

  import { onMount } from "svelte";
  import { app } from "@tauri-apps/api";

  onMount(async () => {
    const menu = await Menu.new({
      items: [
                {
          id: "show",
          text: "show",
          action: () => {
            appWindow.show()
          },
        },
        {
          id: "quit",
          text: "quit",
          action: () => {
            appWindow.close()
          },
        },

      ],
    });
    const options = {
      icon: await defaultWindowIcon(),
      menu,
    };

    const tray = await TrayIcon.new(options);

    console.log(await defaultWindowIcon());
  });

  const appWindow = getCurrentWindow();
</script>

<main class="container font-seurat bg-orange-100 text-stone-900 w-full h-screen">
  <div class="absolute w-full">
    <nav
    class="bg-orange-200 w-full p-1 px-2 flex flex-row wavy-bottom pb-6 cursor justify-between align-middle"
    data-tauri-drag-region
  >
  <div class="flex flex-row items-center space-x-2">
    <img src="/icons/leaf.png" class="size-8 inline" alt="" />
    <p class="text-2xl cursor-default" data-tauri-drag-region>tanuki</p>
  </div>
    
    <div>
      <button
        class="hover:scale-125 transition"
        aria-label="Minimize to tray"
        title="Minimize to tray"
        onclick={appWindow.hide}
      >
        <img src="/icons/down-caret.svg" class="size-8 inline" alt="" />
      </button>
      <button
        class="hover:scale-125 transition"
        aria-label="Close app"
        title="Close app"
        onclick={appWindow.close}
      >
        <img src="/icons/view-close.svg" class="size-8 inline" alt="" />
      </button>
    </div>
  </nav>
  </div>
  

  {@render children()}
</main>

<style>
  .wavy-bottom {
    --mask:
      radial-gradient(35.78px at 50% calc(100% - 48px), #000 99%, #0000 101%)
        calc(50% - 32px) 0/64px 100%,
      radial-gradient(35.78px at 50% calc(100% + 32px), #0000 99%, #000 101%)
        50% calc(100% - 16px) / 64px 100% repeat-x;
    -webkit-mask: var(--mask);
    mask: var(--mask);
  }
</style>
