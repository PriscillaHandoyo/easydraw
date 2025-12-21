<script>
  import { SvelteFlow, Background, Controls, MiniMap } from "@xyflow/svelte";
  import "@xyflow/svelte/dist/style.css";
</script>

<div style="width: 100vw; height: 100vh;">
  <SvelteFlow fitView>
    <Background />
    <Controls />
    <MiniMap />
  </SvelteFlow>
</div>
