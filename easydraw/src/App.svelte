<script>
  import { SvelteFlow, Background, Controls, MiniMap } from "@xyflow/svelte";
  import "@xyflow/svelte/dist/style.css";

  let nodes = $state.raw([
    {
      id: "1",
      position: { x: 0, y: 0 },
      data: { label: "Hello" },
    },
    {
      id: "2",
      position: { x: 100, y: 100 },
      data: { label: "World" },
    },
  ]);
</script>

<div style="width: 100vw; height: 100vh;">
  <SvelteFlow fitView bind:nodes>
    <Background />
    <Controls />
    <MiniMap />
  </SvelteFlow>
</div>
