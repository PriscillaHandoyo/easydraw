<script>
  import { Handle, Position } from "@xyflow/svelte";

  // Take data from the node props
  let { id, data } = $props();

  // Ensure instant update when data.label changes
  // Reduce lagging between pressing a kwy and the text appearing
  let localLabel = $state(data.label);
</script>

<div class="table-node">
  <div class="header">
    <!-- "nodrag" class is used to prevent dragging 
     when highlighting/interacting with the input field 
     "bind:value={localLabel}" is the "double binding" 
     this will make sure when we changed the input, the data will also changed-->
    <input
      type="text"
      bind:value={localLabel}
      oninput={() => {
        // Check if the function exists before calling it
        if (data.onUpdate) {
          data.onUpdate(id, localLabel);
        }
      }}
      class="nodrag"
    />
    <button class="delete-btn nodrag" onclick={() => data.onDelete(id)}>
      x
    </button>
  </div>
  <div class="content">
    <div class="row">ID (PK)</div>
    <div class="row">Created_At</div>
  </div>

  <!-- Hanlde the edges -->
  <Handle type="target" position={Position.Top} />
  <Handle type="source" position={Position.Bottom} />
</div>

<style>
  .table-node {
    background: white;
    border: 2px solid #222;
    border-radius: 4px;
    min-width: 120px;
    font-family: sans-serif;
  }

  .header {
    background: #222;
    color: white;
    padding: 4px 8px;
    font-weight: bold;
    text-align: center;
    display: flex;
  }

  input {
    background: transparent;
    border: none;
    color: white;
    font-weight: bold;
    text-align: center;
    width: 100%;
    outline: none;
  }

  .delete-btn {
    background: transparent;
    border: none;
    color: #ff4d4d;
    cursor: pointer;
    font-size: 18px;
    font-weight: bold;
    padding: 0 5px;
  }

  .delete-btn:hover {
    color: #ff0000;
  }
  .row {
    padding: 4px 8px;
    border-top: 1px solif #eee;
    font-size: 12px;
  }
</style>
