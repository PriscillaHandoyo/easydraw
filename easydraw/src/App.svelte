<script>
  import {
    SvelteFlow,
    Background,
    Controls,
    MiniMap,
    addEdge,
  } from "@xyflow/svelte";
  import "@xyflow/svelte/dist/style.css";

  let nodes = $state.raw([]);

  function addNode() {
    const id = `${nodes.length + 1}`;

    const newNode = {
      id,
      type: "default",
      position: { x: Math.random() * 400, y: Math.random() * 400 },
      data: { label: `Node ${id}` },
    };

    nodes = [...nodes, newNode];
    $: console.log(nodes);
  }

  let edges = $state.raw([]);

  function onConnect(params) {
    edges = addEdge(params, edges);
  }
</script>

<div style="width: 100vw; height: 100vh;">
  <SvelteFlow fitView {nodes} {edges} onconnect={onConnect}>
    <Background />
    <Controls />
    <MiniMap />
  </SvelteFlow>
</div>

<div
  class="controls"
  style="position: absolute; z-index: 10; top: 10px; left: 10px;"
>
  <button onclick={addNode} style="padding: 10px; cursor: pointer;">
    Add New Node
  </button>
</div>
