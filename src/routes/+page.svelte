<script>
  //@ts-nocheck

  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import VinylRecord from '$lib/components/VinylRecord.svelte';
  import { vinyls } from '$lib/vinyls.js';

  let cursorEl;
  let titleEl, subtitleEl;

  onMount(() => {
    // Custom cursor
    const moveCursor = (e) => {
      gsap.to(cursorEl, {
        x: e.clientX,
        y: e.clientY,
        duration: 0.15,
        ease: 'power1.out'
      });
    };
    window.addEventListener('mousemove', moveCursor);

    // Hero entrance
    gsap.fromTo(titleEl,
      { opacity: 0, y: -30 },
      { opacity: 1, y: 0, duration: 1.2, ease: 'power3.out', delay: 0.1 }
    );
    gsap.fromTo(subtitleEl,
      { opacity: 0, y: 20 },
      { opacity: 1, y: 0, duration: 1, ease: 'power2.out', delay: 0.4 }
    );

    return () => window.removeEventListener('mousemove', moveCursor);
  });
</script>

<svelte:head>
  <title>VinylPortfolio</title>
</svelte:head>

<!-- Custom cursor -->
<div
  bind:this={cursorEl}
  class="fixed w-3 h-3 rounded-full bg-[#f0ede6] pointer-events-none z-[9999] mix-blend-difference -translate-x-1/2 -translate-y-1/2"
></div>

<main class="min-h-screen bg-[#0a0a0a] text-[#f0ede6] overflow-x-hidden">

  <!-- Background atmosphere -->
  <div class="fixed inset-0 pointer-events-none z-0" style="
    background:
      radial-gradient(ellipse 60% 50% at 20% 50%, rgba(90,40,10,0.15) 0%, transparent 70%),
      radial-gradient(ellipse 50% 60% at 80% 30%, rgba(30,60,90,0.1) 0%, transparent 70%);
  "></div>

  <!-- Header -->
  <header class="flex justify-between items-center px-12 py-7 relative z-10">
    <span class="font-mono text-[13px] font-bold tracking-[0.2em]">
      VINYL<span class="opacity-35">PORTFOLIO</span>
    </span>
    <nav class="flex gap-8">
      <a href="#about" class="font-mono text-[11px] tracking-[0.15em] uppercase text-[#f0ede6]/50 hover:text-[#f0ede6] transition-colors duration-200 no-underline">
        About
      </a>
      <a href="mailto:hello@example.com" class="font-mono text-[11px] tracking-[0.15em] uppercase text-[#f0ede6]/50 hover:text-[#f0ede6] transition-colors duration-200 no-underline">
        Contact
      </a>
    </nav>
  </header>

  <!-- Hero -->
  <section class="px-12 pt-20 pb-16 max-w-2xl relative z-10" data-scroll data-scroll-speed="0.3">
    <div bind:this={titleEl} class="opacity-0">
      <p class="font-mono text-[10px] tracking-[0.3em] text-[#f0ede6]/35 mb-5">
        A COLLECTION OF WORK
      </p>
      <h1 class="font-serif text-[clamp(42px,6vw,72px)] font-normal leading-[1.05] tracking-[-0.02em] text-[#f0ede6]">
        Each project<br/>
        <em class="text-[#f0ede6]/45">its own world.</em>
      </h1>
    </div>
    <p bind:this={subtitleEl} class="opacity-0 mt-6 text-[15px] leading-relaxed text-[#f0ede6]/40 font-light">
      Hover to pull the record out.<br/>Click to enter the project.
    </p>
  </section>

  <!-- Vinyl shelf -->
  <section class="px-12 pb-24 relative z-10" data-scroll>
    <p class="font-mono text-[10px] tracking-[0.3em] text-[#f0ede6]/20 mb-10">
      — LISTENING TO —
    </p>
    <div class="flex flex-wrap gap-16 items-start">
      {#each vinyls as vinyl, i}
        <div class="flex flex-col gap-4">
          <VinylRecord {vinyl} index={i} />
          <div class="flex items-baseline gap-3">
            <span class="font-mono text-[10px] text-[#f0ede6]/25">0{i + 1}</span>
            <span class="text-[13px] text-[#f0ede6]/45 font-light">{vinyl.title}</span>
            <span class="text-[13px] text-[#f0ede6]/45 font-light">{vinyl.label}</span>
          </div>
        </div>
      {/each}
    </div>
  </section>

  <!-- Footer -->
  <footer class="px-12 py-6 flex justify-between border-t border-[#f0ede6]/[0.06] relative z-10">
    <span class="font-mono text-[10px] tracking-[0.1em] text-[#f0ede6]/20">© 2024 VinylPortfolio</span>
    <span class="font-mono text-[10px] tracking-[0.1em] text-[#f0ede6]/20">Designed & Built with obsession</span>
  </footer>

</main>