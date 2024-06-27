<script>
  import { onMount } from "svelte";

  let canvas;
  let ctx;
  let drawing = false;
  let color = "#ffffff";
  let lineWidth = 5;
  let lineCap = "round";
  let bgColor = "#000000";

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
    ctx.fillStyle = bgColor; // Fill the canvas with the selected background color
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  }

  function setDefaultBackground() {
    ctx.fillStyle = "#000000"; // Default background color
    ctx.fillRect(0, 0, canvas.width, canvas.height);
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

  function updateBgColor(event) {
    bgColor = event.target.value;
    clearCanvas();
  }

  onMount(() => {
    ctx = canvas.getContext("2d");
    setDefaultBackground();
  });
</script>

<div class="mx-auto text-center mb-5 flex justify-evenly w-9/12">
  <div class="flex justify-center">
    <input type="color" value={color} on:input={updateColor} />
    <p class="pl-2 font-bold">Color</p>
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
  <div class="flex justify-center">
    <input type="color" value={bgColor} on:input={updateBgColor} class="mb-5" />
    <p class="pl-2 font-bold">background</p>
  </div>
  <div>
    <button
      on:click={clearCanvas}
      class="btn text-sm bg-green-500 px-5 rounded-3xl text-white py-1 mx-auto hover:bg-slate-600 border-2 border-white"
      >Clean Drawing</button
    >
  </div>
</div>

<div class="w-full flex justify-center">
  <canvas
    class="cursor-crosshair border-4 shadow-2xl rounded-2xl"
    bind:this={canvas}
    width="1000"
    height="600"
    on:mousedown={startDrawing}
    on:mouseup={stopDrawing}
    on:mousemove={draw}
  ></canvas>
</div>
