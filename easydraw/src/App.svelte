<script>
  import { SvelteFlow, Background, Controls, MiniMap } from "@xyflow/svelte";
  import "@xyflow/svelte/dist/style.css";

  let nodes = $state.raw([
    {
      id: "1",
      type: "input",
      position: { x: 0, y: 0 },
      data: { label: "Hello" },
    },
    {
      id: "2",
      type: "output",
      position: { x: 100, y: 100 },
      data: { label: "World" },
    },
  ]);

  let edges = $state.raw([
    {
      id: "e1-2",
      source: "1",
      target: "2",
      type: "smoothstep",
      label: "to the",
    },
  ]);
</script>

<div style="width: 100vw; height: 100vh;">
  <SvelteFlow fitView bind:nodes bind:edges>
    <Background />
    <Controls />
    <MiniMap />
  </SvelteFlow>
</div>
