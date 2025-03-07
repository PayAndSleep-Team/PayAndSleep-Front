<script>
    import { onMount } from "svelte";

    const Api_url = "http://localhost:3000";
    
    // Test data for frontend
    let bills = [
        { description: "ค่าเช่าห้อง", month: "มกราคม", amount: 6000, status: "ยังไม่ชำระ" },
        { description: "ค่าไฟฟ้า", month: "กุมภาพันธ์", amount: 1200, status: "เลยกำหนดชำระ" },
        { description: "ค่าน้ำ", month: "มีนาคม", amount: 500, status: "ชำระแล้ว" },
    ];

    onMount(async () => {
        try {
            const res = await fetch(`${Api_url}/api/bills`, {
                method: "GET",
                credentials: "include",
            });
            bills = await res.json();
        } catch (error) {
            console.error("Error fetching bills:", error);
        }
    });
</script>

<div class="w-full min-h-screen text-white">
    <div class="flex ml-4"><img src="/images/whiteRect.svg" alt="White Rectangle" class="mr-4" /> <h1 class="text-2xl font-bold mb-6 pt-4">ค่าบริการ</h1></div>
    <div class="ml-16 p-4 rounded-lg">
        <div class="space-y-4">
            {#each bills as bill}
                <div class="bg-[#D9D9D9] text-[#404040] p-4 rounded-lg flex justify-between items-center mr-32">
                    <div class="flex items-center">
                        <img src="/images/grayerRect.svg" alt="Gray Rectangle" class="mr-4" /> 
                        <div>
                            <div class="flex items-center space-x-1">
                                <p class="text-xl">รายการค่าบริการ</p>
                                <p class="text-lg">เดือน{bill.month}</p>
                            </div>
                            <div class="flex items-center space-x-1">
                                <p class="text-[#8B8B8C] text-3xl">{bill.amount}</p>
                                <p class="text-[#8B8B8C] text-lg">บาท</p>
                            </div>
                            
                        </div>
                    </div>
                    <span class="px-4 py-2 text-xl rounded-full font-semibold"
                            class:text-[#D0854C]={bill.status === 'ยังไม่ชำระ'}
                            class:text-[#D04C4C]={bill.status === 'เลยกำหนดชำระ'}
                            class:text-[#557B55]={bill.status === 'ชำระแล้ว'}>
                        {bill.status}
                    </span>
                </div>
            {/each}
        </div>
    </div>
</div>