<script>
  //@ts-nocheck
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';

  let { vinyl, index = 0, isOpen = false } = $props();

  let wrapperEl, sleeveEl, recordEl;
  let isHovered = $state(false);

  //Generates 18 numbers between 22 and 68
  const grooves = Array.from({ length: 18 }, (_, i) => 22 + (i / 18) * 46);

  onMount(() => {
    gsap.fromTo(wrapperEl,
      { opacity: 0, y: 60 },
      {
        opacity: 1,
        y: 0,
        duration: 1.1,
        delay: index * 0.2,
        ease: 'power3.out'
      }
    );

    if (isOpen) {
        gsap.fromTo(recordEl, 
          { x: 0, rotate: 0 },
          { x: 250, rotate: 12, duration: 0.8, ease: 'power3.out' }
        );
    }
  });

  function handleMouseEnter() {
    isHovered = true;
    gsap.to(recordEl, { x: 150, rotate: 12, duration: 0.55, ease: 'power3.out' });
    gsap.to(sleeveEl, { y: -6, duration: 0.4, ease: 'power2.out' });
  }

  function handleMouseLeave() {
    isHovered = false;
    gsap.to(recordEl, { x: 0, rotate: 0, duration: 0.55, ease: 'power3.inOut' });
    gsap.to(sleeveEl, { y: 0, duration: 0.4, ease: 'power2.inOut' });
  }

  function handleClick() {
    gsap.to(wrapperEl, {
      scale: 0.95,
      duration: 0.1,
      yoyo: true,
      repeat: 1,
      ease: 'power1.inOut',
      onComplete: () => { window.location.href = vinyl.href; }
    });
  }
</script>

<div bind:this={wrapperEl}
  onmouseenter={handleMouseEnter}
  onmouseleave={handleMouseLeave}
  onclick={handleClick}
  onkeydown={(e) => e.key === 'Enter' && handleClick()}
  role="link"
  tabindex="0"
  class="relative w-[280px] h-[280px] cursor-pointer select-none"
  data-scroll
  data-scroll-speed="0.5">

  <!-- RECORD -->
  <div bind:this={recordEl}
    class="absolute w-[262px] h-[262px] top-[9px] left-[9px] z-[1]"
    style="will-change: transform;">
    <div class="w-full h-full rounded-full relative overflow-hidden"
         style="background: {vinyl.recordBase}; box-shadow: inset 0 0 40px rgba(0,0,0,0.6), 0 4px 20px rgba(0,0,0,0.8);">
      <!-- Groove rings -->
      {#each grooves as pct}
        <div class="groove-ring"
          style="width: {pct * 2}%;
            height: {pct * 2}%;
            border-color: {vinyl.recordGroove};">
        </div>
      {/each}

      <!-- Sheen -->
      <div class="sheen" class:spinning={isHovered}></div>

      <!-- Center label -->
      <div class="absolute w-[36%] h-[36%] rounded-full top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center justify-center gap-[2px] z-[2]"
        style="background: {vinyl.labelBg};">
        <span class="text-[7px] font-bold uppercase tracking-wider text-center" style="color: {vinyl.labelText};">
          {vinyl.label}
        </span>
        <span class="text-[7px] tracking-widest" style="color: {vinyl.labelText}; opacity: 0.6;">
          {vinyl.year}
        </span>
        <!-- Spindle hole -->
        <div class="absolute w-[10px] h-[10px] rounded-full bg-[#0a0a0a] top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"></div>
      </div>

    </div>
  </div>

  <!-- SLEEVE -->
  <div bind:this={sleeveEl}
    class="absolute inset-0 rounded-[4px] z-[2] overflow-hidden"
    style="background: {vinyl.sleeveGradient}; box-shadow: 0 10px 40px rgba(0,0,0,0.5);">
    <div class="absolute inset-0 p-5 flex flex-col justify-between">
      <span class="text-[11px] font-bold tracking-[0.15em] opacity-80" style="color: {vinyl.sleeveAccent};">
        0{vinyl.index}
      </span>
      <div>
        <h2 class="text-[22px] font-bold leading-tight" style="color: {vinyl.sleeveText};">
          {vinyl.title}
        </h2>
        <p class="text-[10px] tracking-[0.2em] uppercase mt-1 opacity-70" style="color: {vinyl.sleeveAccent};">
          {vinyl.subtitle}
        </p>
      </div>
      <div class="h-5">
        {#if isHovered}
          <span class="text-[11px] font-bold tracking-[0.15em] cta-fade" style="color: {vinyl.sleeveText};">
            PLAY →
          </span>
        {/if}
      </div>
    </div>
  </div>

</div>

<style>
  /* Only what Tailwind can't do */

  .groove-ring {
    position: absolute;
    border-radius: 50%;
    border-width: 1px;
    border-style: solid;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .sheen {
    position: absolute;
    inset: 0;
    border-radius: 50%;
    background: conic-gradient(
      from 0deg,
      transparent 0%,
      rgba(255,255,255,0.08) 20%,
      rgba(255,255,255,0.15) 35%,
      transparent 50%,
      transparent 100%
    );
    pointer-events: none;
  }

  .sheen.spinning {
    animation: spinSheen 2s linear infinite;
  }

  @keyframes spinSheen {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }

  .cta-fade {
    animation: fadeIn 0.2s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateX(-4px); }
    to { opacity: 1; transform: translateX(0); }
  }
</style>