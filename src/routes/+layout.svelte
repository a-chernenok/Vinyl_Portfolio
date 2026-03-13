<script>
  //@ts-nocheck
  import '../app.css';
  import { onMount } from 'svelte';
  import LocomotiveScroll from 'locomotive-scroll';

  let { children } = $props();
  let scrollContainer;

  onMount(() => {
    const scroll = new LocomotiveScroll({
      el: scrollContainer,
      smooth: true,
      multiplier: 0.8,
      class: 'is-inview'
    });

    return () => scroll.destroy();
  });
</script>

<div class="min-h-screen" bind:this={scrollContainer}>
  {@render children()}
</div>
