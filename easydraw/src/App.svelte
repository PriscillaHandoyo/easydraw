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

  // Handle array of nodes when the button is clicked
  // Save the new nodes array to the nodes variable
  function addNode() {
    const id = `${nodes.length + 1}`;

    const newNode = {
      id,
      type: "default",
      position: { x: Math.random() * 400, y: Math.random() * 400 },
      data: { label: `Node ${id}` },
    };

    // Update nodes address everytime the button is clicked
    nodes = [...nodes, newNode];
  }

  let edges = $state.raw([]);

  // Handle array of edges when a new connection is made
  // Save the new edges array to the edges variable
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
