<script>
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
  </script>
  
  <div class="p-4 w-full">
    <div class="ml-3">
        <p class="font-semibold text-2xl text-white mb-1">ตรวจสอบรายการชำระเงิน</p>
        <p class="font-bold text-5xl text-white mb-5">{roomNumber}</p>
    </div>
    <div class="grid grid-cols-4 gap-4 mb-4">
      <!-- Green card -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2">
        <div class="flex">
          <div><img src="/images/greenRect.svg" alt="" /></div>
          <div class="flex flex-col">
            <div class="px-3 text-xl text-[#557B55]">ค่าเช่า</div>
            <div
              class="px-3 text-4xl font-bold text-[#557B55]"
            >
              {rent}
            </div>
            <div class="px-3 text-lg text-[#557B55]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Blue card -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2">
        <div class="flex">
          <div><img src="/images/blueRect.svg" alt="" /></div>
          <div class="flex flex-col">
            <div class="px-3 text-xl text-[#546882]">ค่าไฟ</div>
            <div
              class="px-3 text-4xl font-bold text-[#546882]"
            >
              {electricity}
            </div>
            <div class="px-3 text-lg text-[#546882]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Orange card -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2">
        <div class="flex">
          <div><img src="/images/orgRect.svg" alt="" /></div>
          <div class="flex flex-col">
            <div class="px-3 text-xl text-[#D0854C]">ค่าน้ำ</div>
            <div
              class="px-3 text-4xl font-bold text-[#D0854C]"
            >
              {lateFee}
            </div>
            <div class="px-3 text-lg text-[#D0854C]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Red card -->
      <div class="bg-white rounded-xl shadow overflow-hidden p-2">
        <div class="flex">
          <div><img src="/images/redRect.svg" alt="" /></div>
          <div class="flex flex-col">
            <div class="px-3 text-xl text-[#D04C4C]">
              ค่าชำระล่าช้า หรือ อื่นๆ
            </div>
            <div
              class="px-3 text-4xl font-bold text-[#D04C4C]"
            >
              {rent}
            </div>
            <div class="px-3 text-lg text-[#D04C4C]">บาท</div>
          </div>
        </div>
      </div>
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 col-span-2">
        <div class="flex">
          <div><img src="/images/greenRect.svg" alt="" /></div>
          <div class="flex flex-col">
            <div class="px-3 text-xl text-[#557B55]">รวมยอดชำระ</div>
            <div
              class="px-3 text-4xl font-bold text-[#557B55]"
            >
              {total}
            </div>
            <div class="px-3 text-lg text-[#557B55]">บาท</div>
          </div>
        </div>
      </div>
      <div class="bg-white rounded-xl shadow overflow-hidden p-2 col-span-2">
        <div class="flex">
          <div><img src="/images/greenRect.svg" alt="" /></div>
          <div class="flex flex-col">
            <div class="px-3 text-xl text-[#557B55]">วันที่ชำระ</div>
            <div
              class="px-3 text-5xl font-bold text-[#557B55]"
            >
              {date}
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Slip Section -->
    <div class="flex flex-col items-center justify-center shadow rounded-xl bg-[#D9D9D9] py-4 mx-96">
        <img src="/images/slip.png" alt="slip" class="h-5/6"/>
    </div>
    <div class="flex justify-center">
        <div class="flex gap-10 mt-4">
            <button
            class="rounded-2xl text-white bg-[#557B55] py-2 px-12"
            >
            ยืนยัน
            </button>
            <button
            class="rounded-2xl text-white bg-[#D04C4C] py-2 px-12"
            >
            ยกเลิก
            </button>
        </div>
    </div>
  </div>