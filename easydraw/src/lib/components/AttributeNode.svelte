<script>
  import { Handle, Position } from "@xyflow/svelte";

  // Take data from the node props
  let { id, data } = $props();

  // Ensure instant update when data.label changes
  // Reduce lagging between pressing a kwy and the text appearing
  let localLabel = $state(data.label);
</script>

<div class="attribute-node">
  <Handle type="target" position={Position.Top} />

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

  <Handle type="source" position={Position.Bottom} />
</div>

<style>
  .attribute-node {
    padding: 10px 20px;
    border-radius: 50%;
    border: 2px solid #222;
    background: white;
    min-width: 100px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  input {
    border: none;
    background: transparent;
    text-align: center;
    width: 100%;
    outline: none;
  }
</style>
