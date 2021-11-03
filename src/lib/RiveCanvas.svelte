
<script>
  import {Rive, Layout, Fit, Cover, Alignment} from 'rive-js';
  import {createEventDispatcher, onMount} from 'svelte';

  let canvas;

  export let src = "";
  export let animations = [];
  export let stateMachines = [];
  export let triggers = [];
  export let autoplay = false;
  export let fit = Fit.Cover;
  export let alignment = Alignment.TopCenter;
  export let width;
  export let height;

  const dispatch = createEventDispatcher();

  onMount(() => {
    const r = new Rive({
      src,
      canvas,
      fit,
      alignment,
      autoplay,
      animations,
      stateMachines,
      onLoad: (_) => {
        const inputs = r.stateMachineInputs(stateMachines);
        const _trigger = inputs.find(i => i.name === triggers);
        dispatch('load', {
          inputs,
        });
      },
      onStateChange: (e) => {
        const currentAnimation = e.data[0];
        dispatch('state-changed', {
          currentAnimation
        })
      }
    });
  })
</script>


<canvas class="pointer-events-none" bind:this={canvas} {width} {height}></canvas>