<script>
    import { goto } from '$app/navigation';

    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();

    const Api_url = "http://localhost:3000";

    let type = $state(null);
    let description = $state(null);

    let activeIndex = $state(null);

    const setActive = (label, index) => {
        if (label == "ไฟฟ้า") {
            type = "electric";
        } else if (label == "น้ำประปา") {
            type = "water";
        } else if (label == "อุปกรณ์") {
            type = "equipment";
        } else {
            type = "others";
        }
        activeIndex = index;
    }

    const submitRequest = async () => {
        if (!type) {
            alert("กรุณาเลือกประเภท");
            return;
        }
        if (!description) {
            alert("กรุณากรอกรายละเอียด");
            return;
        }
        try {
            const res = await fetch(`${Api_url}/api/create/maintenance-request`, {
                method: "POST",
                credentials: "include",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    type: type,
                    description: description,
                }),
            });
            const data = await res.json();
            if (data.message === "ส่งคำขอซ่อมบำรุงสำเร็จ") {
                alert("ส่งคำร้องสำเร็จ");
                goto("/tenant/dashboard");
            } else {
                alert("ส่งคำร้องไม่สำเร็จ");
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            alert("ส่งคำร้องไม่สำเร็จ");
        }
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
                onclick={() => setActive(label, index)}
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
        <textarea class="rounded-xl bg-[#F2F2F2] h-96 p-5 resize-none" placeholder="แจ้งรายละเอียด..." bind:value={description}>

        </textarea>
    </div>
    <div class="flex justify-center mt-10">
        <button class="rounded-full text-[#F2F2F2] bg-[#404040] py-3 px-8" onclick={submitRequest}>
            ส่งคำร้อง
        </button>
    </div>
</div>