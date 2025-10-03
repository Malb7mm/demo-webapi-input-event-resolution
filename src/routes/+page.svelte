<script lang="ts">
  import { onMount } from "svelte";

  const logs: {
    text: string,
  }[] = $state([]);

  let prev = 0;
  let prev_pf = 0;
  const LOG_MAX = 16;

  onMount(() => {
    document.addEventListener("keydown", (e) => {
      const now = e.timeStamp;
      const now_pf = performance.now();
      const diff = now - prev;
      const diff_pf = now_pf - prev_pf;
      logs.push({
        text: `${now.toFixed(2)} (+${diff.toFixed(2)}) | ${now_pf.toFixed(2)} (+${diff_pf.toFixed(2)})`
      });
      while (logs.length > LOG_MAX) {
        logs.shift();
      }
      prev = now;
      prev_pf = now_pf;
    });
    document.addEventListener("pointerdown", (e) => {
      e.preventDefault();
      const now = e.timeStamp;
      const now_pf = performance.now();
      const diff = now - prev;
      const diff_pf = now_pf - prev_pf;
      logs.push({
        text: `${now.toFixed(2)} (+${diff.toFixed(2)}) | ${now_pf.toFixed(2)} (+${diff_pf.toFixed(2)})`
      });
      while (logs.length > LOG_MAX) {
        logs.shift();
      }
      prev = now;
      prev_pf = now_pf;
    });
    document.addEventListener("pointermove", (e) => {
      e.preventDefault();
    });
  });
</script>
<div>
  <h1>
    入力イベントの分解能確認デモ
  </h1>
  <p>
    Event.timeStamp (+diff) | Performance.now (+diff)
  </p>
  <p>
    キーボード入力/マウス/タッチ どれでも
  </p>
  <hr>
  {#each logs as log}
    <p>
      {log.text}
    </p>
  {/each}
</div>
<style>
  p {
    margin: 0.4rem 0;
  }
  :global(body) {
    touch-action: none;
    overscroll-behavior: none;
    user-select: none;
    -webkit-user-drag: none;
    height: 100vh;
    width: 100vw;
  }
</style>