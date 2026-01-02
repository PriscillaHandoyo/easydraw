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
  import AttributeNode from "./lib/components/AttributeNode.svelte";
  import RelationshipNode from "./lib/components/RelationshipNode.svelte";

  // Const -> DONT CHANGE
  // Define the types of nodes available in the flow
  // Any other types should be added here
  const nodeTypes = {
    table: TableNode,
    attribute: AttributeNode,
    relationship: RelationshipNode,
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
      data: {
        label: `Node ${id}`,
        // Callback function to update the label
        onUpdate: updateNodeLabel,
        onDelete: deleteNode,
      },
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

  // Overwrite the node label
  // Prevent ownership invalid mutation
  function updateNodeLabel(id, newLabel) {
    nodes = nodes.map((n) =>
      n.id === id ? { ...n, data: { ...n.data, label: newLabel } } : n
    );
  }

  // Delete a node based on its ID
  // Use ".filter" to remove the node from the nodes array
  // without having a hole in the array
  function deleteNode(id) {
    nodes = nodes.filter((n) => n.id !== id);
  }

  // Drag the node from the sidebar
  // Set the dataTransfer with the node type and allow move effect
  function onDragStart(event, nodeType) {
    event.dataTransfer.setData("application/svelteflow", nodeType);
    event.dataTransfer.effectAllowed = "move";
  }

  // Drop the node into the canvas
  // Prevent default action and get the node type from dataTransfer
  // Create a new node object and drop it into the nodes array
  function onDrop(event) {
    event.preventDefault();

    const type = event.dataTransfer.getData("application/svelteflow");
    if (!type) return;

    const position = { x: event.clientX - 200, y: event.clientY };

    const newNode = {
      id: `${nodes.length + 1}`,
      type,
      position,
      data: {
        label: `New ${type}`,
        onUpdate: updateNodeLabel,
        onDelete: deleteNode,
      },
    };

    nodes = [...nodes, newNode];
  }

  // Debugging
  // Checking the double binding
  $inspect(nodes);
</script>

<div class="dnd-container">
  <aside class="sidebar">
    <div class="description">Drag these onto the canvas:</div>
    <div
      class="dnd-node table"
      draggable="true"
      ondragstart={(e) => onDragStart(e, "table")}
    >
      Table (Rectangle)
    </div>

    <div
      class="dnd-node attribute"
      draggable="true"
      ondragstart={(e) => onDragStart(e, "attribute")}
    >
      Attribute (Oval)
    </div>

    <div
      class="dnd-node attribute"
      draggable="true"
      ondragstart={(e) => onDragStart(e, "relationship")}
    >
      Relationship (Diamond)
    </div>
  </aside>

  <div class="flow-wrapper" style="width: 100vw; height: 100vh;">
    <SvelteFlow
      fitView
      {nodes}
      {edges}
      {nodeTypes}
      onconnect={onConnect}
      ondrop={onDrop}
      ondragover={(e) => e.preventDefault()}
    >
      <Background />
      <Controls />
      <MiniMap />
    </SvelteFlow>
  </div>
</div>

<style>
  .dnd-container {
    display: flex;
    flex-direction: row;
    width: 100vw;
    height: 100vh;
  }

  .sidebar {
    width: 200px;
    border-right: 1px solid #eee;
    padding: 15px 10px;
    background: #fcfcfc;
    font-family: sans-serif;
  }

  .description {
    font-size: 12px;
    margin-bottom: 10px;
    color: #666;
  }

  .dnd-node {
    height: 40px;
    padding: 4px;
    border: 2px solid #222;
    border-radius: 4px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: grab;
    background: white;
    font-weight: bold;
  }

  .flow-wrapper {
    flex-grow: 1;
    height: 100%;
  }
</style>
