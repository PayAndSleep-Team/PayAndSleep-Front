<script>
    import { onMount } from "svelte";
    import { fade, fly, scale } from "svelte/transition";

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
    
    function getStatusClass(status) {
        switch(status) {
            case 'ยังไม่ชำระ': return 'bg-[#D0854C] bg-opacity-10 text-[#D0854C]';
            case 'เลยกำหนดชำระ': return 'bg-[#D04C4C] bg-opacity-10 text-[#D04C4C]';
            case 'ชำระแล้ว': return 'bg-[#557B55] bg-opacity-10 text-[#557B55]';
            default: return '';
        }
    }
</script>

<div class="w-full min-h-screen text-white p-4 md:p-6" in:fade={{ duration: 300 }}>
    <div class="flex items-center mb-6" in:fly={{ y: -20, duration: 400 }}>
        <div class="w-2 h-12 bg-white mr-4"></div>
        <h1 class="text-2xl md:text-3xl font-bold">ค่าบริการ</h1>
    </div>
    
    <div class="p-10 mx-auto">
        <div class="space-y-3 md:space-y-5">
            {#each bills as bill, i}
                <div 
                    class="bg-[#D9D9D9] text-[#404040] p-2 md:p-3 rounded-xl flex flex-col md:flex-row justify-between items-start md:items-center shadow-md hover:shadow-lg transition-all duration-300 transform hover:-translate-y-1"
                    in:fly={{ y: 20, duration: 300, delay: 100 + i * 100 }}
                >
                    <div class="flex items-center w-full md:w-auto mb-3 md:mb-0">
                        <div class="w-3 h-20 bg-[#8B8B8C] mr-4"></div>
                        <div>
                            <div class="flex flex-col md:flex-row md:items-center md:space-x-2">
                                <p class="text-xl md:text-2xl font-medium">รายการค่าบริการ</p>
                                <p class="text-lg md:text-xl">เดือน{bill.month}</p>
                            </div>
                            <div class="flex items-end space-x-1">
                                <p class="text-[#8B8B8C] text-3xl md:text-4xl font-bold">{bill.amount.toLocaleString()}</p>
                                <p class="text-[#8B8B8C] text-lg md:text-xl mb-1">บาท</p>
                            </div>
                        </div>
                    </div>
                    
                    <span 
                        class={`px-4 py-2 text-base md:text-lg rounded-full font-medium ${getStatusClass(bill.status)}`}
                        in:scale={{ duration: 300, delay: 200 + i * 100 }}
                    >
                        {bill.status}
                    </span>
                </div>
            {/each}
        </div>
        
        {#if bills.length === 0}
            <div 
                class="bg-[#D9D9D9] text-[#8B8B8C] p-8 rounded-lg text-center mt-4"
                in:fade={{ duration: 300, delay: 200 }}
            >
                <p class="text-xl">ไม่พบรายการค่าบริการ</p>
            </div>
        {/if}
    </div>
</div>