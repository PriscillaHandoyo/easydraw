<script>
  import { Handle, Position } from "@xyflow/svelte";

  // Take data from the node props
  let { id, data } = $props();

  // Ensure instant update when data.label changes
  // Reduce lagging between pressing a kwy and the text appearing
  let localLabel = $state(data.label);
</script>

<div class="diamond-container">
  <div class="diamond-shape"></div>

  <div class="content">
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

  <Handle type="target" position={Position.Top} id="t" style="top: -18px" />
  <Handle
    type="source"
    position={Position.Bottom}
    id="s"
    style="bottom: -18px"
  />
  <Handle type="source" position={Position.Left} id="l" style="left: -18px" />
  <Handle type="source" position={Position.Right} id="r" style="right: -18px" />
</div>

<style>
  :global(.svelte-flow__handle) {
    z-index: 10 !important;
    pointer-events: all !important;
  }

  .diamond-container {
    width: 80px;
    height: 80px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  .diamond-shape {
    position: absolute;
    width: 100%;
    height: 100%;
    border: 2px solid #222;
    background: white;
    transform: rotate(45deg);
    z-index: 1;
    pointer-events: none;
  }

  .content {
    position: relative;
    z-index: 2;
    text-align: center;
  }

  input {
    border: none;
    background: transparent;
    text-align: center;
    width: 60px;
    font-size: 10px;
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
</style>
