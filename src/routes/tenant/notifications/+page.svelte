<script>
    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();

    let notifications = [
        "เลยกำหนดการชำระค่าเช่าแล้วเฮ้ยยยยยยยย จ่ายด่วยยยยยยยยยยยยยยยยยยยย",
        "เลยกำหนดการชำระค่าเช่าแล้วเฮ้ยยยยยยยย จ่ายด่วยยยยยยยยยยยยยยยยยยยย",
        "เลยกำหนดการชำระค่าเช่าแล้วเฮ้ยยยยยยยย จ่ายด่วยยยยยยยยยยยยยยยยยยยย",
        "เลยกำหนดการชำระค่าเช่าแล้วเฮ้ยยยยยยยย จ่ายด่วยยยยยยยยยยยยยยยยยยยย",
        "เลยกำหนดการชำระค่าเช่าแล้วเฮ้ยยยยยยยย จ่ายด่วยยยยยยยยยยยยยยยยยยยย"
    ];
</script>

<div class="w-full h-full flex justify-center p-6">
    <div class="w-[90%] h-[90%] p-10 flex flex-col">
        <div class="w-full text-left mb-6">
            <p class="ml-16 mb-8 text-[#F2F2F2] text-2xl font-semibold">
                <span class="text-[#F2F2F2] mr-2">|</span> การแจ้งเตือน
            </p>
        </div>
        <div class="flex-grow overflow-y-auto space-y-6">
            {#each notifications as notification}
                <div class="ml-32 mr-32 flex h-[10%] items-center bg-[#D9D9D9] rounded-lg shadow-md p-4" style="background-color: #D9D9D9; margin-bottom: 43px;">
                    <img src="/images/bell.svg" alt="bell" class="w-6 h-6 mr-4" />
                    <p class="text-lg font-normal" style="color: #404040; margin-left: 1rem;">{notification}</p>
                </div>
            {/each}
        </div>
    </div>
</div>
