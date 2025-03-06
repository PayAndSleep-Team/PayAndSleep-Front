<script>
  import { onMount } from "svelte";
  import { fade, fly, scale } from 'svelte/transition';

  /** @type {{ data: import('./$types').PageData }} */
  let { data } = $props();
  let tenantData = [
    {
      roomNumber: "406-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "409-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "406-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "409-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "406-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "409-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "406-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "409-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
    {
      roomNumber: "406-26",
      name: "ปทิตตา ดวงแก้ว",
      rentStart: "2025-03-10",
      rentEnd: "2026-03-10",
      phone: "00-000-0000",
    },
  ];
</script>

<div class="p-4 md:p-6" in:fade={{ duration: 300 }}>
  <div class="mb-6 md:mb-8">
    <p class="text-2xl md:text-3xl font-semibold text-[#F2F2F2]" in:fly={{ y: -20, duration: 400 }}>
      จัดการสัญญาผู้เช่าใหม่
    </p>
  </div>
  
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6">
    {#each tenantData as tenant, i}
      <div
        class="bg-white rounded-xl shadow overflow-hidden transform transition-all duration-300 hover:scale-[1.02] hover:shadow-lg"
        in:fly={{ y: 20, duration: 400, delay: 150 + (i % 6) * 50 }}
      >
        <div class="flex items-center p-3 pb-0">
          <div class="w-[11px] h-12 bg-[#546882] mr-3"></div>
          <div>
            <div class="text-4xl font-bold text-[#546882]">
              {tenant.roomNumber}
            </div>
          </div>
        </div>
        
        <div class="p-4">
          <div class="grid grid-cols-[auto,1fr] gap-x-2 gap-y-2">
            <div class="text-[#546882] font-medium">ผู้เช่า:</div>
            <div>{tenant.name}</div>
            
            <div class="text-[#546882] font-medium">วันที่เข้าอยู่:</div>
            <div>{new Date(tenant.rentStart).toLocaleDateString('th-TH', {year: 'numeric', month: 'short', day: 'numeric'})}</div>
            
            <div class="text-[#546882] font-medium">วันที่หมดสัญญา:</div>
            <div>{new Date(tenant.rentEnd).toLocaleDateString('th-TH', {year: 'numeric', month: 'short', day: 'numeric'})}</div>
            
            <div class="text-[#546882] font-medium">โทร:</div>
            <div class="flex items-center">
              {tenant.phone}
            </div>
          </div>
          
          <div class="mt-4 flex justify-between">
            <button 
              class="bg-[#557B55] text-white mx-5 px-12 py-1.5 rounded-2xl text-sm hover:bg-opacity-90 transition-all duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-[#557B55] focus:ring-opacity-50"
            >
              ยืนยัน
            </button>
            <button 
              class="bg-[#D04C4C] text-white mx-5 px-12 py-1.5 rounded-2xl text-sm hover:bg-opacity-90 transition-all duration-300 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-[#D04C4C] focus:ring-opacity-50"
            >
              ปฎิเสธ
            </button>
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>
