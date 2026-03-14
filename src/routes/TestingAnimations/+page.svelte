<script>
    //@ts-nocheck
    import { gsap } from 'gsap';
    import { onMount } from 'svelte';

    let box;
    let box2;
    let box3;
    let box4;
    let box5;
    let box6;

    const firstTimeline = () => {
        const tl = gsap.timeline({ 
            defaults: { 
            duration: 1, 
            ease: "power1.inOut"
            }
        });

        tl.to(box, { rotation: 180, x: 300 });
        tl.to(box, { rotation: 0, x: 0 });


        return tl;
    }

    const secondTimeline = () => {
        const tl = gsap.timeline({ 
            defaults: { 
            duration: 1, 
            ease: "power1.inOut"
            }
        });

        tl.to(box2, { rotation: -180, x: 300 });
        tl.to(box2, { rotation: 0, x: 0 });

        return tl;
    }

    const thirdTimeline = () => {
        const tl = gsap.timeline({ 
            defaults: { 
            duration: 1, 
            ease: "power1.inOut"
            }
        });

        tl.to(box3, { rotation: 180, x: 300, scale:2 });
        tl.to(box3, { rotation: 0, x: 0 });

        return tl;
    }

    const rotationAnimation = () => {
        const tl = gsap.timeline({delay: 1, repeat: -1, yoyo:true});

        tl.to(box4, {duration:1, rotation:360})
           .to(box5, {duration: 2, x: -100, ease: 'elastic.out(1, 0.3)'})
           .to(box6, {duration: 2,rotation:360, x:100})
    }

    
    onMount(() => {
        
        rotationAnimation()

        const mainTimeLine = gsap.timeline({
            paused: true,
            repeat: -1,
            yoyo: true
        })
        mainTimeLine.add(firstTimeline())
        .add(secondTimeline(), "-=1")
        .add(thirdTimeline())

        mainTimeLine.play();

        // firstTimeline()
        // secondTimeline()
        // thirdTimeline()
    })
</script>

<div class="h-screen w-full bg-gray-900 text-amber-800">
    <main class="p-10">
        <div class="flex flex-col gap-y-16">
            <div bind:this={box} class="box"><span>B</span></div>
            <div bind:this={box2} class="box"><span>A</span></div>
            <div bind:this={box3} class="box"><span>C</span></div>
        </div>
        <div class="flex flex-col gap-y-16">
            <div bind:this={box4} class="box !bg-purple-200"><span>B</span></div>
            <div bind:this={box5} class="box !bg-purple-200"><span>A</span></div>
            <div bind:this={box6} class="box !bg-purple-200"><span>C</span></div>
        </div>
    </main>
</div>