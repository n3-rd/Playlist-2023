<script>
    export let phoneImage;
    export let trackName;
    export let artistName;
</script>

<div class="container h-[90vh] mx-auto w-[22vw]">
    <div class="phone h-full relative">
        <div class="notch-container absolute top-0 h-[100px] w-full">
            <div
                class="notch w-[20vh] h-[4.25vh] my-0 mx-auto bg-[#111] rounded-b-[2.334vh]"
            />
        </div>
        <img
            class="h-full block border-[1.334vh] border-[#111] rounded-[4.5vh] object-cover object-center"
            src={phoneImage}
            alt=""
        />
        <!-- make an absolute over lay at the bottom for text-->
        <div>
            <div
                class="absolute bottom-0 h-[20vh] w-full bg-[#111] rounded-b-[4.5vh] px-6"
            >
                <div class="h-full w-full flex justify-center items-center">
                    <div class="text-center">
                        <div class="text-[#fff] text-[1.3vw] font-bold">
                            {trackName}
                        </div>
                        <div class="text-[#fff] text-[0.8vw] font-bold">
                            {artistName}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
