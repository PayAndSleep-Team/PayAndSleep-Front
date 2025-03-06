<script>
  import { fade, fly, scale } from 'svelte/transition';
  import { page } from "$app/stores";
  import { onDestroy, onMount } from "svelte";
  import { goto } from '$app/navigation';

  const Api_url = "http://localhost:3000";
  let message = "";

  onMount(async () => {
    const unsubscribe = page.subscribe(($page) => {
        room_id = $page.url.searchParams.get("id") || "";
    });

    onDestroy(unsubscribe);
    try {
      const res = await fetch(`${Api_url}/api/get/expenses?room_id=${room_id}`, {
        method: "GET",
        credentials: "include",
      });
      const data = await res.json();
      expense = data;
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });

  let room_id = "";

  let expense = {};

  // let rent = 4500;
  let water = 0;
  // let waterPerUnit = 18;
  let totalWater = 0;
  let electricity = 0;
  // let electricityPerUnit = 8;
  let totalElectricity = 0;
  let lateFee = 0;
  let isOpen = true;
  // let date = "30/01/67";
  // let roomNumber = "409 - 26";
  let month = new Date().getMonth() + 1;
  let year = new Date().getFullYear();
  
  const closeModal = () => {
    isOpen = false;
  };

  async function createBill() {
    try {
      const res = await fetch(`${Api_url}/api/create/bill`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
        body: JSON.stringify({
          room_id,
          month_year,
          rent_amount: expense.price,
          water_usage: water,
          electricity_usage: electricity,
          water_fee: totalWater,
          electricity_fee: totalElectricity,
          other_fees: lateFee,
          total_amount: total,
        }),
      });
      const data = await res.json();
      if (data.message === "สร้างบิลสำเร็จ") {
        alert("สร้างบิลสำเร็จ");
        goto("/host/room");
      } else {
        message = "Failed to create bill";
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  };
  
  $: month_year = new Date(year, month).toISOString().slice(0, 7);
  $: totalWater = water * expense.water_rate;
  $: totalElectricity = electricity * expense.electricity_rate;
  $: total = expense.price + totalWater + totalElectricity + lateFee;
</script>

<div class="p-4 w-full" in:fade={{ duration: 300 }}>
  <div class="ml-3" in:fly={{ y: -20, duration: 600 }}>
      <p class="font-semibold text-2xl text-white mb-1">แจ้งค่าเช่า</p>
      <p class="font-bold text-5xl text-white mb-5">{expense.room_number}</p>
  </div>
  
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
      <!-- Rent card -->
      <div
        class="bg-white col-span-2 rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 100 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#557B55]"></div>
          <div class="flex flex-col py-2">
            <div class="px-3 text-lg sm:text-xl text-[#557B55]">ค่าเช่า</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
              {expense.price}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Late Fee card -->
      <div
        class="bg-white rounded-xl col-span-2 shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 150 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#D04C4C]"></div>
          <div class="flex flex-col py-2 w-full">
            <div class="px-3 text-lg sm:text-xl text-[#D04C4C]">ค่าชำระล่าช้า หรือ อื่นๆ</div>
            <div class="px-3">
              <input 
                class="text-center w-full text-2xl sm:text-4xl font-bold text-[#D04C4C] rounded-xl bg-[#F2F2F2] bg-opacity-80 px-2 py-1 focus:outline-none focus:ring-2 focus:ring-[#D04C4C] transition-all"
                type="number"
                bind:value={lateFee}
              />
            </div>
            <div class="px-3 text-base sm:text-lg text-[#D04C4C]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Electricity Units card -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 200 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#546882]"></div>
          <div class="flex flex-col py-2 w-full">
            <div class="px-3 text-lg sm:text-xl text-[#546882]">ค่าไฟ</div>
            <div class="px-3">
              <input 
                class="text-center w-full text-2xl sm:text-4xl font-bold text-[#546882] rounded-xl bg-[#F2F2F2] bg-opacity-80 px-2 py-1 focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all"
                type="number"
                bind:value={electricity}
              />
            </div>
            <div class="px-3 text-base sm:text-lg text-[#546882]">หน่วย</div>
          </div>
        </div>
      </div>
  
      <!-- Electricity Total card -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 250 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#546882]"></div>
          <div class="flex flex-col py-2">
            <div class="px-3 text-lg sm:text-xl text-[#546882]">ค่าไฟ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#546882]">{totalElectricity}</div>
            <div class="px-3 text-base sm:text-lg text-[#546882]">บาท</div>
          </div>
        </div>
      </div>
  
      <!-- Water Units card -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 300 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#D0854C]"></div>
          <div class="flex flex-col py-2 w-full">
            <div class="px-3 text-lg sm:text-xl text-[#D0854C]">ค่าน้ำ</div>
            <div class="px-3">
              <input 
                class="text-center w-full text-2xl sm:text-4xl font-bold text-[#D0854C] rounded-xl bg-[#F2F2F2] bg-opacity-80 px-2 py-1 focus:outline-none focus:ring-2 focus:ring-[#D0854C] transition-all"
                type="number"
                bind:value={water}
              />
            </div>
            <div class="px-3 text-base sm:text-lg text-[#D0854C]">หน่วย</div>
          </div>
        </div>
      </div>
  
      <!-- Water Total card -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 350 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#D0854C]"></div>
          <div class="flex flex-col py-2">
            <div class="px-3 text-lg sm:text-xl text-[#D0854C]">ค่าน้ำ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D0854C]">{totalWater}</div>
            <div class="px-3 text-base sm:text-lg text-[#D0854C]">บาท</div>
          </div>
        </div>
      </div>

      <!-- Total card -->
      <div
        class="bg-white col-span-2 rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 400 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#557B55]"></div>
          <div class="flex flex-col py-2">
            <div class="px-3 text-lg sm:text-xl text-[#557B55]">รวมยอดชำระ</div>
            <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
              {total}
            </div>
            <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
          </div>
        </div>
      </div>

      <!-- Month card -->
      <div
        class="bg-white col-span-2 rounded-xl shadow overflow-hidden p-3 transform transition-all duration-300 hover:scale-105"
        in:fly={{ x: -20, duration: 600, delay: 450 }}
      >
        <div class="flex items-center h-full">
          <div class="h-full w-[11px] ml-1 bg-[#557B55]"></div>
          <div class="flex flex-col py-2">
            <div class="px-3 text-lg sm:text-xl text-[#557B55]">ประจำเดือน</div>
            <div class="flex items-center px-3">
              <select 
                bind:value={month}
                class="text-2xl sm:text-4xl font-bold text-[#557B55] bg-[#F2F2F2] bg-opacity-80 rounded-xl px-3 py-1 mr-2 focus:outline-none focus:ring-2 focus:ring-[#557B55]"
              >
                {#each Array(12).fill().map((_, i) => i + 1) as monthNum}
                  <option value={monthNum}>{monthNum}</option>
                {/each}
              </select>
              <span class="text-2xl sm:text-4xl font-bold text-[#557B55]">/ {year}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  
  <!-- Submit Button -->
  <div class="flex justify-center" in:fly={{ y: 20, duration: 600, delay: 500 }}>
      <button
        class="rounded-xl text-white bg-[#557B55] py-3 px-12 text-lg font-semibold transform transition-all duration-300 hover:scale-105 hover:bg-[#446644] focus:outline-none focus:ring-2 focus:ring-[#557B55] focus:ring-offset-2 focus:ring-offset-[#404040]"
        onclick={createBill}
        >
        ยืนยัน
      </button>
  </div>
</div>