<script>
  import RiveCanvas from "$lib/RiveCanvas.svelte";
  import { fade } from 'svelte/transition';

  let triggerText = "Launch";
  let triggerTextVisable = false;
  let trigger;

  let triggerBtnStyles = ""
  let pageStyles = ""
  let launched = false;

  function handleAnimationLoad(e) {

    // Set what trigger to assign
    const inputs = e.detail.inputs;
    trigger = inputs.find(i => i.name === "Trigger_01"); 

  }

  function handleAnimationTrigger() {
    // Trigger rive input to transition to next animation
    trigger.fire();
    launched = !launched;
    
    if(launched) {
      pageStyles = "bg-indigo-500"
    } else {
      pageStyles = ""
    }
    
  }

  function handleAnimationStateChanged(e) {
    if(e.detail.currentAnimation === 'Idle') {
      triggerText = "Launch";
      triggerBtnStyles = "";
    } else if(e.detail.currentAnimation === 'Launched_Idle') {
      triggerText = "Land";
      triggerBtnStyles = "";
      triggerTextVisable = true
    } else {
      triggerBtnStyles = "loading btn-disabled"
      triggerTextVisable = false;
    }
  }

</script>

<main class="h-screen grid content-center justify-center transition duration-1000 {pageStyles}">
  {#if triggerTextVisable}
    <h1 transition:fade class="absolute w-full left-1/2 top-40 transform -translate-x-1/2 -translate-y-1/2 text-primary-content text-center text-6xl font-black tracking-wide">Aim for the stars 🚀</h1>
  {/if}
  <RiveCanvas
    on:load={handleAnimationLoad}
    on:state-changed={handleAnimationStateChanged}
    width="700"
    height="700"
    src="animations/rocketship.riv"
    autoplay={true}
    stateMachines="Rocket_State"
  />
  <div class="flex justify-center">
    <button on:click|preventDefault={handleAnimationTrigger} class="btn {triggerBtnStyles}">{triggerText}</button>
  </div>
</main>