<script>
  import DragAndDrop from '$lib/components/DragAndDrop.svelte';
  export let rent = 4500;
  export let water = 1389;
  export let electricity = 95;
  export let lateFee = 20;
  export let isOpen = true;

  const closeModal = () => {
    isOpen = false;
  };

  $: total = rent + water + electricity + lateFee;
</script>

<div class="p-4 w-full">
  <p class="font-bold text-2xl text-white mb-2">ค่าใช้จ่ายรายเดือน</p>
  <div class="grid grid-cols-4 gap-4 mb-4">
    <!-- Green card -->
    <div class="bg-white rounded-xl shadow overflow-hidden p-2">
      <div class="flex">
        <div><img src="/images/greenRect.svg" alt="" /></div>
        <div class="flex flex-col">
          <div class="px-3 text-xl" style="color: #557B55">ค่าเช่า</div>
          <div
            class="px-3 text-4xl font-bold text-[#557B55]"
            style="color: #557B55"
          >
            {rent}
          </div>
          <div class="px-3 text-lg" style="color: #557B55">บาท</div>
        </div>
      </div>
    </div>

    <!-- Blue card -->
    <div class="bg-white rounded-xl shadow overflow-hidden p-2">
      <div class="flex">
        <div><img src="/images/blueRect.svg" alt="" /></div>
        <div class="flex flex-col">
          <div class="px-3 text-xl" style="color: #546882">ค่าไฟ</div>
          <div
            class="px-3 text-4xl font-bold text-[#546882]"
            style="color: #546882"
          >
            {electricity}
          </div>
          <div class="px-3 text-lg" style="color: #546882">บาท</div>
        </div>
      </div>
    </div>

    <!-- Orange card -->
    <div class="bg-white rounded-xl shadow overflow-hidden p-2">
      <div class="flex">
        <div><img src="/images/orgRect.svg" alt="" /></div>
        <div class="flex flex-col">
          <div class="px-3 text-xl" style="color: #D0854C">ค่าน้ำ</div>
          <div
            class="px-3 text-4xl font-bold text-[#D0854C]"
            style="color: #D0854C"
          >
            {lateFee}
          </div>
          <div class="px-3 text-lg" style="color: #D0854C">บาท</div>
        </div>
      </div>
    </div>

    <!-- Red card -->
    <div class="bg-white rounded-xl shadow overflow-hidden p-2">
      <div class="flex">
        <div><img src="/images/redRect.svg" alt="" /></div>
        <div class="flex flex-col">
          <div class="px-3 text-xl" style="color: #D04C4C">
            ค่าชำระล่าช้า หรือ อื่นๆ
          </div>
          <div
            class="px-3 text-4xl font-bold text-[#D04C4C]"
            style="color: #D04C4C"
          >
            {rent}
          </div>
          <div class="px-3 text-lg" style="color: #D04C4C">บาท</div>
        </div>
      </div>
    </div>
    <div class="bg-white rounded-xl shadow overflow-hidden p-2 col-span-2">
      <div class="flex">
        <div><img src="/images/greenRect.svg" alt="" /></div>
        <div class="flex flex-col">
          <div class="px-3 text-xl" style="color: #557B55">รวมยอดชำระ</div>
          <div
            class="px-3 text-4xl font-bold text-[#557B55]"
            style="color: #557B55"
          >
            {total}
          </div>
          <div class="px-3 text-lg" style="color: #557B55">บาท</div>
        </div>
      </div>
    </div>
  </div>
  <!-- qrCode section -->
  <div
    class="flex flex-col items-center justify-center shadow rounded-xl text-white relative"
    style="padding-bottom: 1rem;"
  >
    <p class="text-2xl mt-4 font-semibold">ชำระเงิน</p>
    <div class="p-8 rounded-xl" stlye="background: rgb(217, 217, 217);">
        <DragAndDrop />
    </div>
    {#if isOpen}
        <div class="p-5 absolute bg-[#404040] rounded-xl" style="margin-top: 10rem;">
            <button onclick="{closeModal}" class="absolute right-0 top-0">
                <img src="/images/cross.svg" alt="cross" />
            </button>
            
            <div class="text-center">
                <p class="text-xl">พร้อมเพย์</p>
                <p class="text-xl mb-2">0958232112</p>
            </div>
            
            <img src="/images/qrcode.jpg" alt="qrCode" style="width: 20em" />
        </div>
    {/if}
    <div class="flex gap-4 mt-4">
      <button
        class="rounded-xl text-[#404040] bg-white"
        style="padding: 10px 30px;"
      >
        ยืนยัน
      </button>
      <button
        class="rounded-xl text-[#404040] bg-white"
        style="padding: 10px 30px;"
      >
        ยกเลิก
      </button>
    </div>
  </div>
</div>