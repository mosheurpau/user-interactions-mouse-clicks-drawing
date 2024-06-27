<script>
  import { onMount } from "svelte";

  let canvas;
  let ctx;
  let drawing = false;
  let color = "#ffffff";
  let lineWidth = 5;
  let lineCap = "round";

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

    ctx.lineWidth = lineWidth;
    ctx.lineCap = lineCap;
    ctx.strokeStyle = color;

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

  function updateColor(event) {
    color = event.target.value;
  }

  function updateLineWidth(event) {
    lineWidth = event.target.value;
  }

  function updateLineCap(event) {
    lineCap = event.target.value;
  }

  onMount(() => {
    ctx = canvas.getContext("2d");
  });
</script>

<div class="mx-auto text-center mb-5 flex justify-evenly w-10/12">
  <div>
    <input
      type="color"
      value={color}
      on:input={updateColor}
      class="h-10 w-15"
    />
  </div>
  <div>
    <input
      type="range"
      min="1"
      max="80"
      value={lineWidth}
      on:input={updateLineWidth}
      class="mb-5"
    />
    <label>{lineWidth}px</label>
  </div>
  <div>
    <select on:change={updateLineCap} class="mb-5">
      <option value="butt">Butt</option>
      <option value="round" selected>Round</option>
      <option value="square">Square</option>
    </select>
  </div>
  <div>
    <button
      on:click={clearCanvas}
      class="btn bg-green-500 px-5 rounded-3xl text-white py-2 mx-auto hover:bg-slate-600 border-2 border-y-green-900"
      >Clean Drawing</button
    >
  </div>
</div>
<canvas
  class="mx-auto cursor-crosshair border-4 shadow-2xl bg-black rounded-2xl"
  bind:this={canvas}
  width="1000"
  height="600"
  on:mousedown={startDrawing}
  on:mouseup={stopDrawing}
  on:mousemove={draw}
></canvas>
