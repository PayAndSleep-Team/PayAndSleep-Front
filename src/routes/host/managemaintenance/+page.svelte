<script>
    let newRequests = [
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้าหน้าหี", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้าหน้าหี", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-27", type: "ประปา", description: "น้ำรั่วใต้ซิงค์", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้า", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },
        { room: "409-26", type: "ไฟฟ้าหน้าหี", description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง", status: "ดำเนินการ" },

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
</script>

<div class="w-screen flex flex-col px-8">
    <h1 class="text-xl text-[#F2F2F2] font-medium mb-4 mt-3">จัดการคำร้องซ่อมบำรุง</h1>

    <div class="w-full p-6 rounded-lg text-[#F2F2F2]">
        <h2 class="text-lg font-bold border-b border-gray-400 pb-2 flex items-center">
            <img src="/images/whiteRect.svg" width="10" height="10" class="mr-2" alt="สี่เหลี่ยมสีขาว"> 
            คำร้องใหม่
        </h2>
        <div class="flex overflow-x-auto gap-4 mt-2 pb-4 w-[88%]">
            {#each newRequests as req}
                <div class="bg-[#F2F2F2] text-black p-4 rounded-lg shadow flex-shrink-0 flex flex-col justify-center items-center" style="min-width: 300px; height: 200px;">
                    <p class="text-lg font-bold text-center">{req.room}</p>
                    <p class="text-sm border border-black px-2 py-1 inline-block rounded-md mt-2">{req.type}</p>
                    <p class="text-xs text-[#8B8B8C] mb-3 mt-3 text-center">{req.description}</p>
                    <button class="px-6 py-2 rounded-md text-xs mt-2 inline-block" style="background-color: #557B55; color: white;">{req.status}</button> 
                </div>
            {/each}
        </div>

        <h2 class="text-lg font-bold border-b text-[#F2F2F2] border-gray-400 pb-2 mt-6 flex items-center">
            <img src="/images/whiteRect.svg" width="10" height="10" class="mr-2" alt="สี่เหลี่ยมสีขาว"> 
            กำลังดำเนินการ
        </h2>
        <div class="flex overflow-x-auto gap-4 mt-2 pb-4 w-[88%]">
            {#each inProgressRequests as req}
                <div class="bg-[#F2F2F2] text-black p-4 rounded-lg shadow flex-shrink-0 flex flex-col justify-center items-center" style="min-width: 300px; height: 200px;">
                    <p class="text-lg font-bold text-center">{req.room}</p>
                    <p class="text-sm border border-black px-2 py-1 inline-block rounded-md mt-2">{req.type}</p>
                    <p class="text-xs mb-3 mt-3 text-[#8B8B8C] text-center">{req.description}</p>
                    <button class="px-6 py-2 rounded-md text-xs mt-2 inline-block" style="background-color: #546882; color: white;">{req.status}</button> 
                </div>
            {/each}
        </div>

        <h2 class="text-lg font-bold border-b text-[#F2F2F2] border-gray-400 pb-2 mt-6 flex items-center">
            <img src="/images/whiteRect.svg" width="10" height="10" class="mr-2" alt="สี่เหลี่ยมสีขาว"> 
            สำเร็จแล้ว
        </h2>
        <div class="flex overflow-x-auto gap-4 mt-2 pb-4 w-[88%]">
            {#each completedRequests as req}
                <div class="bg-[#F2F2F2] text-black p-4 rounded-lg shadow flex-shrink-0 flex flex-col justify-center items-center" style="min-width: 300px; height: 200px;">
                    <p class="text-lg font-bold text-center">{req.room}</p>
                    <p class="text-sm border border-black px-2 py-1 inline-block rounded-md mt-2">{req.type}</p>
                    <p class="text-xs text-[#8B8B8C] mb-3 mt-3 text-center">{req.description}</p>
                </div>
            {/each}
        </div>
    </div>
</div>