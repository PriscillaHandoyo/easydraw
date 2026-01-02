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
  // let nodes = $state([]);

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
  //let edges = $state([]);

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

  function clearCanvas() {
    if (
      confirm(
        "Are you sure you want to clear the whole canvas? This cannot be undone."
      )
    ) {
      nodes = [];
      edges = [];
    }
  }

  // Load the data from LocalStorage when the app starts
  // localStorage -> Web API
  const savedNodes = localStorage.getItem("easy-draw-nodes");
  const savedEdges = localStorage.getItem("easy-draw-edges");

  // Intialize state with saved data or an empty array if nothing exists
  // If there's data, turns the string (JSON) back to real JavaScript array or object that can be use by Svelte Flow
  let nodes = $state.raw(savedNodes ? JSON.parse(savedNodes) : []);
  let edges = $state.raw(savedEdges ? JSON.parse(savedEdges) : []);

  // Automatically save whenever nodes or edges change
  // Give the lable and turn the array into strings (JSON)
  $effect(() => {
    localStorage.setItem("easy-draw-nodes", JSON.stringify(nodes));
    localStorage.setItem("easy-draw-edges", JSON.stringify(edges));
  });

  // Debugging
  // Checking the double binding
  $inspect(nodes);
</script>

<div class="dnd-container">
  <nav class="navbar">
    <div class="logo">EasyDraw</div>
    <div class="actions">
      <button class="clear-btn" onclick={clearCanvas}> Clear Canvas </button>
    </div>
  </nav>

  <main class="app-body">
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
  </main>
</div>

<style>
  .navbar {
    height: 50px;
    background: #333;
    color: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 20px;
  }

  .clear-btn {
    background: #ff4d4d;
    color: white;
    border: none;
    padding: 8px 15px;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
  }

  .clear-btn:hover {
    background: #cc0000;
  }

  .app-body {
    display: flex;
    flex-direction: row;
    height: calc(100ch - 50px);
    width: 100vw;
  }

  .sidebar {
    width: 200px;
    height: 100%;
    border-right: 1px solid #eee;
    padding: 10px;
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
