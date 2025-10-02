<script lang="ts">
  import { onMount } from "svelte";

  const logs: {
    text: string,
  }[] = $state([]);

  let prev = 0;
  const LOG_MAX = 12;

  onMount(() => {
    document.addEventListener("keydown", (e) => {
      const now = Math.floor(e.timeStamp);
      const diff = now - prev;
      logs.push({
        text: `${now} (+${diff})`
      });
      while (logs.length > LOG_MAX) {
        logs.shift();
      }
      prev = now;
    });
    document.addEventListener("pointerdown", (e) => {
      const now = Math.floor(e.timeStamp);
      const diff = now - prev;
      logs.push({
        text: `${now} (+${diff})`
      });
      while (logs.length > LOG_MAX) {
        logs.shift();
      }
      prev = now;
    });
  });
</script>
<div>
  <h1>
    入力イベントの分解能確認デモ
  </h1>
  <p>
    Event.timeStampを取得して表示しています
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
</style>