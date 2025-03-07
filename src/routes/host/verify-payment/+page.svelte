<script>
    import { fade, fly } from "svelte/transition";
    
    export let rent = 4500;
    export let water = 1389;
    export let electricity = 95;
    export let lateFee = 20;
    export let isOpen = true;
    export let date = "30/01/67";
    export let roomNumber = "409 - 26"
    const closeModal = () => {
      isOpen = false;
    };
  
    $: total = rent + water + electricity + lateFee;
    
    function handleConfirm() {
      alert("ยืนยันการชำระเงินสำเร็จ");
    }
    
    function handleCancel() {
      alert("ยกเลิกการชำระเงิน");
    }
</script>
  
<div class="p-4 w-full min-h-screen" in:fade={{ duration: 300 }}>
    <div class="ml-3 mb-6" in:fly={{ y: -20, duration: 400 }}>
        <p class="font-semibold text-2xl text-white mb-1">ตรวจสอบรายการชำระเงิน</p>
        <p class="font-bold text-4xl md:text-5xl text-white">{roomNumber}</p>
    </div>
    
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6" in:fly={{ y: 20, duration: 400, delay: 200 }}>
      <!-- Green card - Rent -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105">
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
          <div class="flex flex-col">
            <div class="px-3 text-lg sm:text-xl text-[#557B55]">ค่าเช่า</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
              {rent}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Blue card - Electricity -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105">
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 my-2 bg-[#546882]"></div>
          <div class="flex flex-col">
            <div class="px-3 text-lg sm:text-xl text-[#546882]">ค่าไฟ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#546882]">
              {electricity}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#546882]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Orange card - Water -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105">
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 my-2 bg-[#D0854C]"></div>
          <div class="flex flex-col">
            <div class="px-3 text-lg sm:text-xl text-[#D0854C]">ค่าน้ำ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D0854C]">
              {water}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#D0854C]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Red card - Late Fee -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105">
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 my-2 bg-[#D04C4C]"></div>
          <div class="flex flex-col">
            <div class="px-3 text-lg sm:text-xl text-[#D04C4C]">
              ค่าชำระล่าช้า หรือ อื่นๆ
            </div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D04C4C]">
              {lateFee}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#D04C4C]">บาท</div>
          </div>
        </div>
      </div>
      
      <!-- Total Amount -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 sm:col-span-1 md:col-span-2 transform transition-all duration-300 hover:scale-105">
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
          <div class="flex flex-col">
            <div class="px-3 text-lg sm:text-xl text-[#557B55]">รวมยอดชำระ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
              {total}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
          </div>
        </div>
      </div>
      
      <!-- Payment Date -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 sm:col-span-1 md:col-span-2 transform transition-all duration-300 hover:scale-105">
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
          <div class="flex flex-col">
            <div class="px-3 text-lg sm:text-xl text-[#557B55]">วันที่ชำระ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
              {date}
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Slip Section -->
    <div class="flex flex-col items-center justify-center shadow rounded-xl bg-[#D9D9D9] py-6 mx-auto max-w-md md:max-w-lg lg:max-w-xl mb-6" in:fly={{ y: 20, duration: 400, delay: 300 }}>
        <img src="/images/slip.png" alt="slip" class="max-h-[500px] w-auto"/>
    </div>
    
    <div class="flex justify-center" in:fly={{ y: 20, duration: 400, delay: 400 }}>
        <div class="flex flex-col sm:flex-row gap-4 sm:gap-10 mt-4">
            <button
              class="rounded-2xl text-white bg-[#557B55] py-2 px-12 transform transition-all duration-300 hover:scale-105 hover:bg-[#446644] focus:outline-none focus:ring-2 focus:ring-[#557B55] focus:ring-offset-2 focus:ring-offset-[#404040]"
              on:click={handleConfirm}
            >
              ยืนยัน
            </button>
            <button
              class="rounded-2xl text-white bg-[#D04C4C] py-2 px-12 transform transition-all duration-300 hover:scale-105 hover:bg-[#b33e3e] focus:outline-none focus:ring-2 focus:ring-[#557B55] focus:ring-offset-2 focus:ring-offset-[#404040]"
              on:click={handleCancel}
            >
              ยกเลิก
            </button>
        </div>
    </div>
</div>