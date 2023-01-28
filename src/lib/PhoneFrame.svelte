<script>
    export let phoneImage;
    export let trackName;
    export let artistName;
</script>

<div class="w-screen h-screen flex justify-center items-center">
    <div class="w-[39vw] text-[5rem] text-center serif font-black">
        {artistName}
    </div>
    <div class="container h-[90vh] mx-auto w-[22vw]">
        <div class="phone h-full relative">
            <div class="notch-container absolute top-0 h-[100px] w-full">
                <div
                    class="notch w-[20vh] h-[4.25vh] my-0 mx-auto bg-[#111] rounded-b-[2.334vh]"
                />
            </div>
            <img
                class="h-full block border-[1.334vh] border-[#111] rounded-[4.5vh] object-cover object-center transition duration-500 linear"
                src={phoneImage}
                alt=""
            />
            <!-- make an absolute over lay at the bottom for text-->
        </div>
    </div>
    <div class="w-[39vw] text-[5rem] text-center serif font-black">
        {trackName}
    </div>
</div>
