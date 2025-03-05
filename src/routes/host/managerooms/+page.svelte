<script>
    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();
    let isEditing = $state(false);

    let user = $state({});
    let profile = $state({});
    let tempSize = $state("459 x 4645 ตารางนิ้ว")
    let tempPrice = $state("6500 บาท/เดือน");
    let tempAvailable = $state("ว่าง");
    function toggleEdit() {
      if (!isEditing) {
        tempSize = profile?.name;
        tempPrice = user?.email;
        tempAvailable = user?.password;
      } else {
        if (profile) {
          profile.name = tempSize;
        }
        if (user) {
          user.email = tempPrice;
          user.password = tempAvailable;
        }
      }
      isEditing = !isEditing;
    }
</script>

<div class="w-full grid grid-cols-2" >
    <img src="/images/mrbg.svg" class="col-span-2 w-[95%]" alt="roompic"/>
    <div class="ml-16 mt-4 text-[#F2F2F2] flex flex-col h-full w-full">
        <p class="text-[#F2F2F2] font-normal font-jeju text-xl">ขนาด</p>
        {#if isEditing}
            <input class="ml-6 mb-4 py-1 px-6 rounded-xl border-2  border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-lg font-normal" bind:value={tempSize} placeholder="ขนาดห้อง"/>
        {:else}
        <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">{tempSize}</p>
        {/if}
        <div class="w-full border-t-2 border-[#F2F2F2]"></div>
        <p class="text-[#F2F2F2] font-normal font-jeju text-xl">ราคา</p>
        {#if isEditing}
            <input class="ml-6 mb-4 py-1 px-6 rounded-xl border-2  border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-lg font-normal" bind:value={tempPrice} placeholder="ราคาห้องต่อเดือน"/>
        {:else}
            <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">6500 บาท/เดือน</p>
        {/if}
        <div class="w-full border-t-2 border-[#F2F2F2]"></div>
        <p class=" text-[#F2F2F2] font-normal font-jeju text-xl">สถานะ</p>
        {#if isEditing}
            <input class="ml-6 mb-4 py-1 px-6 rounded-xl border-2  border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-lg font-normal" bind:value={tempPrice} placeholder="สถานะห้อง"/>
        {:else}
            <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">ว่าง</p>
        {/if}
        <div class="w-full border-t-2 border-[#F2F2F2]"></div>
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
    
</div>