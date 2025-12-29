<script>
  import {
    SvelteFlow,
    Background,
    Controls,
    MiniMap,
    addEdge,
  } from "@xyflow/svelte";
  import "@xyflow/svelte/dist/style.css";
  import TableNode from "./lib/components/TableNode.svelte";

  // Const -> DONT CHANGE
  // Define the types of nodes available in the flow
  // Any other types should be added here
  const nodeTypes = {
    table: TableNode,
  };

  // State -> Changeable
  let nodes = $state([]);

  // Handle array of nodes when the button is clicked
  // Save the new nodes array to the nodes variable
  function addNode() {
    const id = `${nodes.length + 1}`;

    const newNode = {
      id,
      type: "table",
      position: { x: Math.random() * 400, y: Math.random() * 400 },
      data: { label: `Node ${id}` },
    };

    // Update nodes address everytime the button is clicked
    nodes = [...nodes, newNode];
  }

  // State -> Changeable
  let edges = $state([]);

  // Handle array of edges when a new connection is made
  // Save the new edges array to the edges variable
  function onConnect(params) {
    edges = addEdge(params, edges);
  }

  // Debugging
  // Checking the double binding
  $inspect(nodes);
</script>

<div style="width: 100vw; height: 100vh;">
  <SvelteFlow fitView {nodes} {edges} {nodeTypes} onconnect={onConnect}>
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
