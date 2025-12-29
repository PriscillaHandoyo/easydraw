<script>
  import { Handle, Position } from "@xyflow/svelte";
  export let data; // Take data from the node props
</script>

<div class="table-node">
  <div class="header">
    <!-- "nodrag" class is used to prevent dragging 
     when highlighting/interacting with the input field -->
    <input type="text" bind:value={data.label} class="nodrag" />
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
  input {
    background: transparent;
    border: none;
    color: white;
    font-weight: bold;
    text-align: center;
    width: 100%;
    outline: none;
  }
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
  }

  .row {
    padding: 4px 8px;
    border-top: 1px solif #eee;
    font-size: 12px;
  }
</style>
