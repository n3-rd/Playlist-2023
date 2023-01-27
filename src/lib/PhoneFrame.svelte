<script>
    export let phoneImage;
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
    </div>
</div>
