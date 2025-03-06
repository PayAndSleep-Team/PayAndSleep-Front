<script>
    import { goto } from '$app/navigation';
    import { fade, fly } from 'svelte/transition';
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

<div class="min-h-screen p-4" in:fade={{ duration: 300 }}>
    <div class="flex items-center mx-4 sm:mx-8 md:mx-12 lg:mx-16 my-8" in:fly={{ y: -20, duration: 600 }}>
        <img src="/images/whiteRect.svg" alt="whiteRect" class="w-6 h-6 md:w-8 md:h-8">
        <p class="text-xl md:text-2xl ml-2 text-[#F2F2F2]">ส่งคำขอซ่อมบำรุง</p>
    </div>
    
    <div class="flex flex-col mx-4 sm:mx-8 md:mx-12 lg:mx-20 mb-6" in:fly={{ x: -20, duration: 800 }}>
        <p class="text-[#F2F2F2] text-lg md:text-xl mb-3">เลือกประเภท</p>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-3 md:gap-6">
            {#each ['ไฟฟ้า', 'น้ำประปา', 'อุปกรณ์', 'อื่นๆ'] as label, index}
                <button
                    onclick={() => setActive(label, index)}
                    in:fly={{ y: 20, duration: 600, delay: index * 150 }}
                    class="text-base md:text-xl rounded-lg px-4 md:px-6 py-2 md:py-3 font-medium shadow-md transition-all duration-300 hover:scale-105
                        {activeIndex === index 
                        ? 'bg-[#404040] text-[#F2F2F2]' 
                        : 'bg-[#D9D9D9] text-[#404040] hover:bg-[#BFBFBF]'}"
                >
                    {label}
                </button>
            {/each}
        </div>
    </div>

    <div class="flex flex-col mx-4 sm:mx-8 md:mx-12 lg:mx-20" in:fly={{ x: 20, duration: 800 }}>
        <p class="text-[#F2F2F2] text-lg md:text-xl mb-3">รายละเอียด</p>
        <textarea 
            class="rounded-xl bg-[#F2F2F2] h-48 md:h-96 p-5 resize-none transition-all duration-300 focus:ring-2 focus:ring-[#404040] outline-none" 
            placeholder="แจ้งรายละเอียด..." 
            bind:value={description}
        ></textarea>
    </div>

    <div class="flex justify-center mt-6 md:mt-10" in:fly={{ y: 20, duration: 800 }}>
        <button 
            class="rounded-full text-[#F2F2F2] bg-[#404040] py-2 md:py-3 px-6 md:px-8 transition-all duration-300 hover:scale-105 hover:bg-[#333333]" 
            onclick={submitRequest}
        >
            ส่งคำร้อง
        </button>
    </div>
</div>