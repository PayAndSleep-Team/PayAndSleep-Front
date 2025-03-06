<script>
    let isEditing = false;
    let tempSize = "459 x 4645";
    let tempPrice = "6500";
    let tempAvailable = "ว่าง";

    function toggleEdit() {
        isEditing = !isEditing;
    }
</script>

<div class="w-full relative flex flex-col items-center">
    <div class="relative w-[95%]">
        <img src="/images/mrbg.svg" class="w-full rounded-lg" alt="roompic"/>
        <div class="ml-6 absolute bottom-4 left-6 text-white">
            <p class="text-xl font-medium">จัดการห้องพัก</p>
            <p class="text-4xl font-bold">409 - 26</p>
        </div>
    </div>

    <div class="ml-32 mt-4 text-[#F2F2F2] flex flex-col h-full w-full">
        <p class="mb-2 text-[#F2F2F2] font-normal font-jeju text-xl">ขนาด</p>
        {#if isEditing}
            <input class="ml-6 mb-4 py-1 px-6 rounded-xl border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] text-[#404040] w-[30%] font-jeju text-lg font-normal" bind:value={tempSize} placeholder="ขนาดห้อง"/>
        {:else}
            <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">{tempSize} ตารางเมตร</p>
        {/if}

        <div class="w-[80%] border-t-2 border-[#F2F2F2]"></div>

        <p class="mb-2 text-[#F2F2F2] font-normal font-jeju text-xl">ราคา</p>
        {#if isEditing}
            <input class="ml-6 mb-4 py-1 px-6 rounded-xl border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] text-[#404040] w-[30%] font-jeju text-lg font-normal" bind:value={tempPrice} placeholder="ราคาห้องต่อเดือน"/>
        {:else}
            <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">{tempPrice}  บาท/เดือน</p>
        {/if}

        <div class="w-[80%] border-t-2 border-[#F2F2F2]"></div>

        <p class=" mb-2 text-[#F2F2F2] font-normal font-jeju text-xl">สถานะ</p>
        {#if isEditing}
            <input class="ml-6 mb-4 py-1 px-6 rounded-xl border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] text-[#404040] w-[30%] font-jeju text-lg font-normal" bind:value={tempAvailable} placeholder="สถานะห้อง"/>
        {:else}
            <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">{tempAvailable}</p>
        {/if}

        <div class="w-[80%] border-t-2 border-[#F2F2F2]"></div>
    </div>
</div>
<button onclick={toggleEdit} class="absolute bottom-12 right-12 border-none p-0">
    {#if isEditing}
        <div class="w-12 h-12 flex items-center justify-center">
            <img src="/images/confirm.svg" alt="Confirm" class="w-8 h-8" />
        </div>
    {:else}
        <div class="w-12 h-12 bg-[url('/images/editbg.svg')] bg-cover flex items-center justify-center">
            <img src="/images/edit.svg" alt="Edit" class="w-8 h-8" />
        </div>
    {/if}
</button>