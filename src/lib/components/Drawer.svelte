<script>
  //@ts-nocheck
  import { gsap } from 'gsap';
  import { onMount } from 'svelte';

  let { vinyl, onClose } = $props();

  let drawerEl;

  onMount(() => {
    gsap.fromTo(drawerEl,
      { x: '100%' },
      { x: '0%', duration: 0.6, ease: 'power3.out' }
    );
  });

  function close() {
    gsap.to(drawerEl, {
      x: '100%',
      duration: 0.5,
      ease: 'power3.inOut',
      onComplete: onClose
    });
  }
</script>

<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="fixed inset-0 z-50" onclick={close}>

  <!-- Backdrop -->
   <!-- backdrop-blur-sm -->
  <div class="absolute inset-0 "></div>

  <!-- Drawer panel -->
  <div bind:this={drawerEl}
    class="absolute right-0 top-0 h-full w-[920px] bg-[#111] border-l border-[#f0ede6]/10 p-12 flex flex-col gap-10 overflow-y-auto"
    onclick={(e) => e.stopPropagation()}>

    <!-- Close button -->
    <button onclick={close} class="self-end font-mono text-[11px] tracking-[0.2em] text-[#f0ede6]/40 hover:text-[#f0ede6] transition-colors">
      CLOSE ×
    </button>

    <!-- Vinyl index -->
    <span class="font-mono text-[10px] tracking-[0.3em] text-[#f0ede6]/25">
      0{vinyl.index} — {vinyl.subtitle}
    </span>

    <!-- Title -->
    <div class="flex flex-col gap-3">
      <h2 class="font-serif text-[48px] font-normal leading-none tracking-tight text-[#f0ede6]">
        {vinyl.title}
      </h2>
      <span class="font-mono text-[11px] tracking-[0.2em] text-[#f0ede6]/40">
        {vinyl.label} · {vinyl.year}
      </span>
    </div>

    <!-- Divider -->
    <div class="w-12 h-px bg-[#f0ede6]/20"></div>

    <!-- Description -->
    <p class="text-[15px] leading-relaxed text-[#f0ede6]/60 font-light">
      {vinyl.description}
    </p>

    <!-- Links -->
    <div class="flex flex-col gap-4 mt-auto">
      {#each vinyl.links as link}
          <a href={link.url}
          target="_blank"
          class="flex justify-between items-center py-4 border-t border-[#f0ede6]/10 text-[#f0ede6]/60 hover:text-[#f0ede6] transition-colors group">
          <span class="font-mono text-[12px] tracking-[0.15em]">{link.label}</span>
          <span class="text-[18px] group-hover:translate-x-1 transition-transform">→</span>
        </a>
      {/each}
    </div>

  </div>
</div>