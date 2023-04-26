<script>
  import { onMount } from "svelte";

  const commentCandidates = [
    "なんだって！？",
    "衝撃の事実！！",
    "信じられない！！",
    "ジンギスカン食べたい！",
  ];
  let comment =
    commentCandidates[Math.floor(Math.random() * commentCandidates.length)];

  const loadImage = (canvas) => {
    return new Promise((resolve, reject) => {
      const image = new Image();
      image.src = "/cat.png";
      image.onload = () => {
        let ctx = canvas.getContext("2d");
        canvas.width = image.width;
        canvas.height = image.height;
        ctx.drawImage(image, 0, 0);
        resolve();
      };
      image.onerror = reject;
    });
  };

  const drawText = (canvas, text) => {
    let ctx = canvas.getContext("2d");

    ctx.fillStyle = "rgb(255, 255, 255)";
    ctx.fillRect(0, 0, canvas.width, 110);

    ctx.font = "bold 42px sans-serif";
    ctx.fillStyle = "rgb(234, 88, 12)";
    ctx.textAlign = "center";
    ctx.textBaseline = "middle";
    ctx.fillText(text, canvas.width / 2, 60);
  };

  $: {
    let canvas = document.getElementById("cat");
    if (canvas) drawText(canvas, comment);
  }

  const download = () => {
    let canvas = document.getElementById("cat");
    const a = document.createElement("a");
    a.href = canvas.toDataURL("image/png");
    a.download = "suzuki-cat.png";
    a.click();
  };

  const copy = () => {
    let canvas = document.getElementById("cat");
    canvas.toBlob((blob) => {
      const item = new ClipboardItem({ "image/png": blob });
      navigator.clipboard.write([item]);
    });
  };

  onMount(async () => {
    const canvas = document.getElementById("cat");
    await loadImage(canvas);
    drawText(canvas, comment);
  });
</script>

<div
  class="max-w-4xl m-1 p-1 h-screen flex flex-col gap-y-12 justify-center mx-auto"
>
  <main>
    <div class="m-auto">
      <div class="flex flex-col items-center gap-y-4">
        <div class="font-bold text-2xl text-slate-600 font-mono">
          Suzuki Cat Generator
        </div>
        <div class="hover:animate-ping">
          <canvas id="cat" />
        </div>
        <div class="mt-2">
          <textarea
            bind:value={comment}
            name="comment"
            id="comment"
            rows="1"
            class="block w-72 rounded p-3 text-orange-600 font-bold shadow-sm border-1.5 border-gray-700"
          />
        </div>
        <div class="text-sm flex gap-x-2">
          <button
            type="button"
            on:click={copy}
            class="rounded-md bg-gray-700 px-3 py-1 shadow text-white hover:bg-orange-600"
            >クリップボードへコピー</button
          >
          <button
            type="button"
            on:click={download}
            class="rounded-md bg-gray-700 px-3 py-1 shadow text-white hover:bg-orange-600"
            >ダウンロード</button
          >
        </div>
      </div>
    </div>
  </main>

  <footer class="text-center text-xs font-mono text-gray-500">
    <div>
      original artwork: <a
        class="underline hover:text-orange-600"
        href="https://twitter.com/yuskesuzki/status/1651109487485550595"
        >@yusukesuzuki</a
      >
    </div>
    <div>
      code: <a
        class="underline hover:text-orange-600"
        href="https://github.com/sorami/suzuki-cat-generator"
        >github.com/sorami/suzuki-cat-generator</a
      >
    </div>
  </footer>
</div>
