<script>
  import { onMount } from "svelte";

  let canvas;
  let ctx;
  let drawing = false;

  function startDrawing(event) {
    drawing = true;
    draw(event);
  }

  function stopDrawing() {
    drawing = false;
    ctx.beginPath();
  }

  function draw(event) {
    if (!drawing) return;

    ctx.lineWidth = 5;
    ctx.lineCap = "round";
    ctx.strokeStyle = "#000000";

    ctx.lineTo(
      event.clientX - canvas.offsetLeft,
      event.clientY - canvas.offsetTop
    );
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(
      event.clientX - canvas.offsetLeft,
      event.clientY - canvas.offsetTop
    );
  }

  function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  }

  onMount(() => {
    ctx = canvas.getContext("2d");
  });
</script>

<div class="mx-auto text-center">
  <button
    on:click={clearCanvas}
    class="btn bg-green-500 px-5 rounded-3xl text-white py-2 mb-5 mx-auto hover:bg-slate-600 border-2 border-y-green-900"
    >Clean Drawing</button
  >
</div>
<canvas
  class="mx-auto cursor-crosshair border-4 shadow-2xl"
  bind:this={canvas}
  width="1000"
  height="600"
  on:mousedown={startDrawing}
  on:mouseup={stopDrawing}
  on:mousemove={draw}
></canvas>
