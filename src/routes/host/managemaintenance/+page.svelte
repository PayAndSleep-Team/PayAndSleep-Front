<script>
    import { onMount } from "svelte";
    import { fade, fly, scale } from "svelte/transition";
    
    let newRequests = [
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
    ];

    let inProgressRequests = [
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "เสร็จสิ้น" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "เสร็จสิ้น" },
    ];

    let completedRequests = [
        { room: "409-26", type: "ประปา", description: "แก้ไขปัญหาน้ำรั่วใต้ซิงค์แล้ว" },
        { room: "409-27", type: "ไฟฟ้า", description: "เปลี่ยนหลอดไฟใหม่เรียบร้อย" },
        { room: "409-28", type: "อื่นๆ", description: "ตรวจสอบและแก้ไขปัญหาเรียบร้อย" }
    ];
    
    let activeTab = "new";

    function moveToInProgress(index) {
        const request = newRequests[index];
        request.status = "เสร็จสิ้น";
        inProgressRequests = [...inProgressRequests, request];
        newRequests = newRequests.filter((_, i) => i !== index);
    }

    function moveToCompleted(index) {
        const request = inProgressRequests[index];
        const { status, ...completedRequest } = request;
        completedRequests = [...completedRequests, completedRequest];
        inProgressRequests = inProgressRequests.filter((_, i) => i !== index);
    }
</script>

<div class="w-full min-h-screen p-4 md:p-6">
    <h1 class="text-2xl md:text-3xl text-[#F2F2F2] font-bold mb-6" in:fade={{ duration: 400, delay: 100 }}>
        จัดการคำร้องซ่อมบำรุง
    </h1>

    <!-- Tab navigation for mobile -->
    <div class="md:hidden flex mb-4 bg-[#2A2A2A] rounded-lg overflow-hidden" in:fade={{ duration: 300 }}>
        <button 
            class="flex-1 py-2 px-4 text-center text-sm {activeTab === 'new' ? 'bg-[#404040] text-white' : 'text-gray-300'}"
            on:click={() => activeTab = 'new'}>
            คำร้องใหม่
        </button>
        <button 
            class="flex-1 py-2 px-4 text-center text-sm {activeTab === 'inProgress' ? 'bg-[#404040] text-white' : 'text-gray-300'}"
            on:click={() => activeTab = 'inProgress'}>
            กำลังดำเนินการ
        </button>
        <button 
            class="flex-1 py-2 px-4 text-center text-sm {activeTab === 'completed' ? 'bg-[#404040] text-white' : 'text-gray-300'}"
            on:click={() => activeTab = 'completed'}>
            สำเร็จแล้ว
        </button>
    </div>

    <div class="w-full rounded-lg text-[#F2F2F2]">
        <!-- New Requests Section -->
        <div class="{activeTab === 'new' || activeTab === '' ? 'block' : 'hidden md:block'}">
            <h2 class="text-lg md:text-xl font-bold pb-2 flex items-center" in:fade={{ duration: 300 }}>
                <div class="h-12 w-2 bg-[#D9D9D9] mr-2"></div>
                คำร้องใหม่
            </h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 mt-4 pb-10 border-b border-[#A4A5A6]">
                {#each newRequests as req, i}
                    <div 
                        class="bg-[#F2F2F2] text-black p-4 rounded-2xl shadow flex flex-col justify-between transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1 h-[200px]"
                        in:fly={{ y: 20, duration: 300, delay: 150 + i * 50 }}
                        out:fly={{ y: -20, duration: 300 }}
                    >
                        <div>
                            <p class="text-2xl font-bold text-center">{req.room}</p>
                            <p class="text-md border border-black px-4 py-1 rounded-xl mt-2 mx-auto text-center w-fit">{req.type}</p>
                            <p class="text-sm text-[#8B8B8C] my-3 mt-5 text-center line-clamp-2">{req.description}</p>
                        </div>
                        <button 
                            class="px-6 py-2 rounded-xl text-sm mx-auto bg-[#557B55] text-white hover:bg-[#446644] transition-colors"
                            on:click={() => moveToInProgress(i)}
                        >
                            {req.status}
                        </button> 
                    </div>
                {/each}
            </div>
        </div>

        <!-- In Progress Requests Section -->
        <div class="{activeTab === 'inProgress' ? 'block' : 'hidden md:block'} mt-8">
            <h2 class="text-lg md:text-xl font-bold pb-2 flex items-center" in:fade={{ duration: 300 }}>
                <div class="h-12 w-2 bg-[#D9D9D9] mr-2"></div>
                กำลังดำเนินการ
            </h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 mt-4 pb-10 border-b border-[#A4A5A6]">
                {#each inProgressRequests as req, i}
                    <div 
                        class="bg-[#F2F2F2] text-black p-4 rounded-2xl shadow flex flex-col justify-between transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1 h-[200px]"
                        in:fly={{ y: 20, duration: 300, delay: 150 + i * 50 }}
                        out:fly={{ y: -20, duration: 300 }}
                    >
                        <div>
                            <p class="text-2xl font-bold text-center">{req.room}</p>
                            <p class="text-md border border-black px-4 py-1 rounded-xl mt-2 mx-auto text-center w-fit">{req.type}</p>
                            <p class="text-sm text-[#8B8B8C] my-3 mt-5 text-center line-clamp-2">{req.description}</p>
                        </div>
                        <button 
                            class="px-6 py-2 rounded-xl text-sm mx-auto bg-[#546882] text-white hover:bg-[#435771] transition-colors"
                            on:click={() => moveToCompleted(i)}
                        >
                            {req.status}
                        </button> 
                    </div>
                {/each}
            </div>
        </div>

        <!-- Completed Requests Section -->
        <div class="{activeTab === 'completed' ? 'block' : 'hidden md:block'} mt-8">
            <h2 class="text-lg md:text-xl font-bold pb-2 flex items-center" in:fade={{ duration: 300 }}>
                <div class="h-12 w-2 bg-[#D9D9D9] mr-2"></div>
                คำร้องที่เสร็จสิ้น
            </h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 mt-4 pb-10">
                {#each completedRequests as req, i}
                    <div 
                        class="bg-[#F2F2F2] text-black p-4 rounded-2xl shadow flex flex-col justify-between transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1 h-[200px]"
                        in:fly={{ y: 20, duration: 300, delay: 150 + i * 50 }}
                        out:fly={{ y: -20, duration: 300 }}
                    >
                        <div>
                            <p class="text-2xl font-bold text-center">{req.room}</p>
                            <p class="text-md border border-black px-4 py-1 rounded-xl mt-2 mx-auto text-center w-fit">{req.type}</p>
                            <p class="text-sm text-[#8B8B8C] my-3 mt-5 text-center line-clamp-2">{req.description}</p>
                        </div>
                    </div>
                {/each}
            </div>
        </div>
    </div>
</div>