<script>
    import { onMount } from "svelte";
    import { gsap } from "gsap";
    import "splitting/dist/splitting.css";
    import "splitting/dist/splitting-cells.css";
    import Splitting from "splitting";

    onMount(() => {
        Splitting();
        const tl = gsap.timeline();

        tl.from(".loading-text .char", {
            translateY: -400,
            stagger: 0.1,
            duration: 0.5,
            ease: "elastic",
        }).to(".bar-inner", {
            width: "100%",
        });
    });
</script>

<div
    class="h-screen w-screen z-[999] fixed bg-[#C2C2C2] flex justify-center items-center"
>
    <div class="loader">
        <div
            class="loading-text text-center serif text-7xl h-fit py-4 overflow-hidden"
            data-splitting
        >
            Loading
        </div>
        <div class="loading-bar py-5">
            <div class="w-[30vw] h-2 bg-[#272727] ">
                <div class="bar-inner h-full w-1/6 bg-[#C2C2C2]" />
            </div>
        </div>
    </div>
</div>
