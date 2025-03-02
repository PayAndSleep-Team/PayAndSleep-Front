<script>
    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();
</script>

<div class="w-full h-full flex items-center justify-center p-6">
    <div class="w-[90%] h-[90%] p-10 flex flex-col">
        <div class="w-full text-left mb-6">
            <p class="text-[#F2F2F2] text-2xl font-semibold">
                <span class="text-[#F2F2F2] mr-2">|</span>ส่งคำร้องซ่อมบำรุง
            </p>
        </div>

        <div class="flex-grow flex flex-col items-center"> 
            <div class="mb-6 w-[60%]"> 
                <p class="text-[#F2F2F2] text-xl font-semibold mb-2">เลือกประเภท</p>
                <div class="flex justify-between gap-4">
                    <button class="w-[23%] border border-[#404040] rounded-lg px-6 py-3 text-[#404040] bg-[#D9D9D9] font-medium hover:bg-[#404040] hover:text-[#F2F2F2] shadow-md">ไฟฟ้า</button>
                    <button class="w-[23%] border border-[#404040] rounded-lg px-6 py-3 text-[#404040] bg-[#D9D9D9] font-medium hover:bg-[#404040] hover:text-[#F2F2F2] shadow-md">น้ำปะปา</button>
                    <button class="w-[23%] border border-[#404040] rounded-lg px-6 py-3 text-[#404040] bg-[#D9D9D9] font-medium hover:bg-[#404040] hover:text-[#F2F2F2] shadow-md">อุปกรณ์</button>
                    <button class="w-[23%] border border-[#404040] rounded-lg px-6 py-3 text-[#404040] bg-[#D9D9D9] font-medium hover:bg-[#404040] hover:text-[#F2F2F2] shadow-md">อื่นๆ</button>
                </div>
            </div>

            <div class="mb-6 w-[60%] flex-grow flex flex-col"> 
                <p class="text-[#F2F2F2] text-xl font-semibold mb-2">รายละเอียด</p>
                <textarea class="w-full h-full border bg-[#D9D9D9] border-[#404040] rounded-lg p-3 text-[#404040] focus:outline-none focus:ring focus:border-[#000000] placeholder:text-[#A4A5A6]" placeholder="แจ้งรายละเอียด..."></textarea>
            </div>

            <div class="w-full flex justify-center"> 
                <button class="w-[10%] py-3 mt-9 rounded-[25px] text-xl cursor-pointer bg-[#404040] border-none text-[#D9D9D9] shadow-md">ส่งคำร้อง</button>
            </div>
        </div>
    </div>
</div>