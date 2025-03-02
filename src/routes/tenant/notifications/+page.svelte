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

<div class="w-full h-screen bg-cover bg-center bg-no-repeat flex items-center justify-center p-6"
     style="background-image: url('/images/bg.png');">
    <div class="w-[90%] h-[90%] p-10 flex flex-col">
        <div class="w-full text-left mb-6">
            <p class="ml-16 mb-8 text-[#F2F2F2] text-2xl font-semibold">
                <span class="text-[#F2F2F2] mr-2">|</span> การแจ้งเตือน
            </p>
        </div>
        <div class="flex-grow overflow-y-auto space-y-6"> <!-- Changed space-y-4 to space-y-6 -->
            {#each notifications as notification}
                <div class="ml-32 mr-32 flex h-[10%] items-center bg-[#D6D6D6] rounded-lg shadow-md p-4">
                    <img src="/images/bell.svg" alt="bell" class="w-6 h-6 mr-4" />
                    <p class="text-[#404040] text-lg font-normal">{notification}</p>
                </div>
            {/each}
        </div>
    </div>
</div>