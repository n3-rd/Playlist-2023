<script>
    import { onMount } from "svelte";
    import PhoneFrame from "./PhoneFrame.svelte";
    import Controls from "./Controls.svelte";
    import ColorThief from "colorthief";
    import { Howl, Howler } from "howler";
    import Preloader from "./Preloader.svelte";
    let mainScreen;
    let playlistData = [];
    let currentTrack = 0;
    let controlsColor = "#fff";
    let loaded = false;
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

        chnageBackground(
            playlistData[currentTrack]?.track?.album?.images[0]?.url
        );
    };
    const previousTrack = () => {
        currentTrack =
            currentTrack === 0 ? playlistData.length - 1 : currentTrack - 1;

        chnageBackground(
            playlistData[currentTrack]?.track?.album?.images[0]?.url
        );
    };
    const chnageBackground = (imageUrl) => {
        // @ts-ignore
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
        // @ts-ignore
        sound.play();
    };

    const stopSound = () => {
        Howler.stop();
    };
    onMount(async () => {
        await fetchPlayList().then(() => {
            loaded = true;
            chnageBackground(
                playlistData[currentTrack]?.track?.album?.images[0]?.url
            );
        });
        console.log("Hello from Svelte!");
    });
</script>

{#if loaded}
    <div
        class="h-screen w-screen flex justify-center items-center overflow-x-hidden bg-[#3E628E] transition duration-500 linear"
        bind:this={mainScreen}
    >
        <div class="phone-frame z-[777]" style="color: {controlsColor}">
            <PhoneFrame
                phoneImage={playlistData[currentTrack]?.track?.album?.images[0]
                    ?.url}
                trackName={playlistData[currentTrack]?.track?.name}
                artistName={playlistData[currentTrack]?.track?.artists[0]?.name}
            />
        </div>

        <div
            class="absolute bottom-8 right-8 flex justify-between items-center z-[777]"
        >
            <Controls
                playPreviousTrack={previousTrack}
                songToPlay={playlistData[currentTrack]?.track.preview_url}
                stopSoundButton={stopSound}
                playNextTrack={nextTrack}
                controlsColorButton={controlsColor}
            />
        </div>
    </div>
{:else}
    <div class="preloader z-[999] fixed h-screen w-screen">
        <Preloader />
    </div>
{/if}
