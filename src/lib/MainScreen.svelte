<script>
    import { onMount } from "svelte";
    import PhoneFrame from "./PhoneFrame.svelte";
    import CircleType from "circletype";
    import ColorThief from "colorthief";
    import { Howl, Howler } from "howler";
    import NextButton from "./NextButton.svelte";
    import PreviousButton from "./PreviousButton.svelte";
    import PlayButton from "./PlayButton.svelte";
    import StopButton from "./StopButton.svelte";
    let circleText;
    let mainScreen;
    let playlistData = [];
    let currentTrack = 0;
    let controlsColor = "#fff";
    const fetchPlayList = async () => {
        try {
            const response = await fetch(
                "https://rich-cyan-sturgeon-sock.cyclic.app/getPlaylist?playlistId=6vOaJlrNbsjDCa6kuSw3K6"
            );
            const data = await response.json();
            playlistData = data.tracks.items;
            playlistData.sort(() => Math.random() - 0.5);
            console.log(playlistData);
        } catch (e) {
            console.log(e);
        }
    };
    const nextTrack = () => {
        currentTrack =
            currentTrack === playlistData.length - 1 ? 0 : currentTrack + 1;
        // update the current track name
        circleText.innerHTML = playlistData[currentTrack]?.track?.name;
        new CircleType(circleText);
        chnageBackground(
            playlistData[currentTrack]?.track?.album?.images[0]?.url
        );
    };
    const previousTrack = () => {
        currentTrack =
            currentTrack === 0 ? playlistData.length - 1 : currentTrack - 1;
        circleText.innerHTML = playlistData[currentTrack]?.track?.name;
        new CircleType(circleText);
        chnageBackground(
            playlistData[currentTrack]?.track?.album?.images[0]?.url
        );
    };
    const chnageBackground = (imageUrl) => {
        const colorThief = new ColorThief();
        const img = new Image();
        img.crossOrigin = "Anonymous";
        img.src = imageUrl;
        img.onload = () => {
            const color = colorThief.getColor(img);
            console.log(color);
            mainScreen.style.backgroundColor = `rgb(${color[0]}, ${color[1]}, ${color[2]})`;
            // make the controls color change to a contrasting color
            controlsColor =
                color[0] + color[1] + color[2] > 382
                    ? "rgba(0, 0, 0, 0.8)"
                    : "rgba(255, 255, 255, 0.8)";
        };
    };
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

    const stopSound = () => {
        Howler.stop();
    };
    onMount(async () => {
        await fetchPlayList();
        console.log("Hello from Svelte!");
        new CircleType(circleText);
        chnageBackground(
            playlistData[currentTrack]?.track?.album?.images[0]?.url
        );
    });
</script>

<div
    class="h-screen w-screen flex justify-center items-center overflow-x-hidden bg-[#3E628E]"
    bind:this={mainScreen}
>
    <div class="phone-frame z-[999]">
        <PhoneFrame
            phoneImage={playlistData[currentTrack]?.track?.album?.images[0]
                ?.url}
        />
    </div>

    <div
        class="circle-text absolute text-[6rem] serif font-black uppercase"
        style="color: {controlsColor};"
        bind:this={circleText}
    >
        {playlistData[currentTrack]?.track?.name}
    </div>

    <div class="absolute bottom-8 right-8 flex justify-between items-center">
        <button on:click={previousTrack}>
            <PreviousButton fill={controlsColor} />
        </button>
        <button
            on:click={playSound(playlistData[currentTrack]?.track.preview_url)}
        >
            <PlayButton fill={controlsColor} />
        </button>
        <button on:click={stopSound}>
            <StopButton fill={controlsColor} />
        </button>
        <button on:click={nextTrack}>
            <NextButton fill={controlsColor} />
        </button>
    </div>
</div>

<style>
    .circle-text {
        animation: circle-text 30s infinite;
    }
    @keyframes circle-text {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
</style>
