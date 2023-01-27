<script>
    export let controlsColorButton;
    export let songToPlay;
    export let playNextTrack;
    export let playPreviousTrack;
    export let stopSoundButton;
    import NextButton from "./NextButton.svelte";
    import PreviousButton from "./PreviousButton.svelte";
    import PlayButton from "./PlayButton.svelte";
    import StopButton from "./StopButton.svelte";
    import { Howl, Howler } from "howler";
    const playSound = (url) => {
        const sound = new Howl({
            src: [url],
            html5: true,
            onplay: function () {
                console.log("onplay");
            },
            onend: function () {
                console.log("Finished!");
            },
        });
        sound.play();
    };
</script>

<button on:click={playPreviousTrack}>
    <PreviousButton fill={controlsColorButton} />
</button>
<button on:click={playSound(songToPlay)}>
    <PlayButton fill={controlsColorButton} />
</button>
<button on:click={stopSoundButton}>
    <StopButton fill={controlsColorButton} />
</button>
<button on:click={playNextTrack}>
    <NextButton fill={controlsColorButton} />
</button>
