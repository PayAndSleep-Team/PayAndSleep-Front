<script>
    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();
    let activeIndex = $state(null);

    const setActive = (index) => {
        activeIndex = index;
    }
</script>

<div class="">
    <div class="flex items-center" style="margin: 2rem;">
        <img src="/images/whiteRect.svg" alt="whiteRect">
        <p class="text-2xl ml-2 text-[#F2F2F2]">ส่งคำขอซ่อมบำรุง</p>
    </div>
    <div class="flex flex-col mx-20 mb-6">
        <p class="text-[#F2F2F2] text-xl mb-3">เลือกประเภท</p>
        <div class="grid grid-cols-4 gap-6">
            {#each ['ไฟฟ้า', 'น้ำประปา', 'อุปกรณ์', 'อื่นๆ'] as label, index}
                <button
                onclick={() => setActive(index)}
                class="text-xl rounded-lg px-6 py-3 font-medium shadow-md transition
                    {activeIndex === index 
                    ? 'bg-[#404040] text-[#F2F2F2]' 
                    : 'bg-[#D9D9D9] text-[#404040]'}"
                >
                {label}
                </button>
            {/each}
        </div>
    </div>
    <div class="flex flex-col mx-20">
        <p class="text-[#F2F2F2] text-xl mb-3">รายละเอียด</p>
        <textarea class="rounded-xl bg-[#F2F2F2] h-96 p-5 resize-none" placeholder="แจ้งรายละเอียด...">

        </textarea>
    </div>
</div>