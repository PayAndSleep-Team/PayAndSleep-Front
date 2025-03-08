<script>
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";
  import { fade, fly, scale } from 'svelte/transition';

  const Api_url = "http://localhost:3000";
  let properties = $state([]);
  let rooms = $state([]);
  let showRoomSelection = $state(false);
  let contractDuration = $state({ months: "", years: "" });
  let selectedRoom = $state("");
  let selectedProperty = $state("");
  let start_date = $state("");
  let isLoading = $state(false);
  let errorMessage = $state("");
  let room = $derived(rooms.find((r) => r.room_number === selectedRoom));

  onMount(async () => {
    isLoading = true;
    try {
      const res = await fetch(`${Api_url}/api/get/property`, {
        method: "GET",
        credentials: "include",
      });
      properties = await res.json();
    } catch (error) {
      console.error("Error fetching properties:", error);
      errorMessage = "ไม่สามารถโหลดข้อมูลที่พักได้";
    } finally {
      isLoading = false;
    }
  });

  const getRooms = async () => {
    if (!selectedProperty) {
      errorMessage = "กรุณาเลือกที่พัก";
      return;
    }
    
    errorMessage = "";
    isLoading = true;
    try {
      showRoomSelection = true;
      const prop = properties.find((p) => p.name === selectedProperty);
      const res = await fetch(
        `${Api_url}/api/get/rooms?property_id=${prop.id}`,
        {
          method: "GET",
          credentials: "include",
        },
      );
      rooms = await res.json();
    } catch (error) {
      console.error("Error fetching rooms:", error);
      errorMessage = "ไม่สามารถโหลดข้อมูลห้องพักได้";
      showRoomSelection = false;
    } finally {
      isLoading = false;
    }
  };

  const createRentalContract = async () => {
    if (!selectedRoom) {
      errorMessage = "กรุณาเลือกห้อง";
      return;
    }
    if (!start_date) {
      errorMessage = "กรุณาเลือกวันที่เข้าอยู่";
      return;
    }
    if (!contractDuration.years) {
      errorMessage = "กรุณากรอกระยะเวลาของสัญญา";
      return;
    }
    
    errorMessage = "";
    isLoading = true;
    try {
      const room = rooms.find((r) => r.room_number === selectedRoom);
      const res = await fetch(`${Api_url}/api/create/rental-contract`, {
        method: "POST",
        credentials: "include",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          room_id: room.id,
          start_date: formatDateTime(start_date),
          end_date: endDate(),
        }),
      });
      const data = await res.json();
      
      if (data.message === "สร้างสัญญาเช่าสำเร็จ") {
        showSuccessMessage();
        setTimeout(() => goto("/landing/waiting"), 1500);
      } else {
        errorMessage = data.message;
      }
    } catch (error) {
      console.error("Error creating rental contract:", error);
      errorMessage = "ไม่สามารถสร้างสัญญาเช่าได้";
    } finally {
      isLoading = false;
    }
  };

  function formatDateTime(dateStr) {
    let date = new Date(dateStr);
    let year = date.getFullYear();
    let month = String(date.getMonth() + 1).padStart(2, '0');
    let day = String(date.getDate()).padStart(2, '0');
    let hours = String(date.getHours()).padStart(2, '0');
    let minutes = String(date.getMinutes()).padStart(2, '0');
    let seconds = String(date.getSeconds()).padStart(2, '0');

    return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
  }

  function endDate() {
    let date = new Date(start_date);
    date.setFullYear(date.getFullYear() + Number(contractDuration.years));
    date.setMonth(date.getMonth() + Number(contractDuration.months || 0));
    return formatDateTime(date);
  }
  
  function showSuccessMessage() {
    const successMessage = document.createElement('div');
    successMessage.className = 'fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50';
    successMessage.innerHTML = `
      <div class="bg-white p-6 rounded-xl shadow-lg flex flex-col items-center">
        <svg class="w-16 h-16 text-green-500 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
        </svg>
        <p class="text-xl font-medium">สร้างสัญญาเช่าสำเร็จ</p>
        <p class="text-gray-500 mt-2">กำลังนำคุณไปยังหน้ารอการยืนยัน...</p>
      </div>
    `;
    document.body.appendChild(successMessage);
    setTimeout(() => {
      document.body.removeChild(successMessage);
    }, 1500);
  }
  
  function goBack() {
    showRoomSelection = false;
    selectedRoom = "";
    errorMessage = "";
  }
</script>

<div class="w-full h-screen flex justify-center md:justify-end items-center px-4 md:pr-[5%]" in:fade={{ duration: 300 }}>
  {#if !showRoomSelection}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-full sm:w-[80%] md:w-[60%] lg:w-[45%] h-auto py-10 md:h-[95%] shadow-lg flex flex-col items-center justify-center p-5"
      in:scale={{ duration: 400, delay: 100 }}
    >
      <h1
        class="text-[#404040] text-center font-jeju text-3xl md:text-4xl font-normal mb-8"
        in:fly={{ y: -20, duration: 400, delay: 200 }}
      >
        เลือกที่พัก
      </h1>
      
      {#if errorMessage}
        <div 
          class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-xl mb-4 w-[90%] max-w-[550px]"
          in:fly={{ y: 20, duration: 300 }}
        >
          <p>{errorMessage}</p>
        </div>
      {/if}
      
      <form class="w-full flex flex-col items-center" in:fly={{ y: 20, duration: 400, delay: 300 }}>
        <div class="relative w-full sm:w-[80%] md:w-[60%] mb-8 transition-all duration-300 hover:scale-[1.01]">
          <select
            bind:value={selectedProperty}
            required
            class="w-full py-3 px-5 rounded-xl border-2 border-[#404040] bg-transparent text-left appearance-none pr-10 {selectedProperty
              ? 'text-[#404040]'
              : 'text-[#8B8B8C]'} font-jeju text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          >
            <option value="" disabled selected>ที่พัก</option>
            {#each properties as property}
              <option value={property.name} class="text-[#404040]"
                >{property.name}</option
              >
            {/each}
          </select>
          <div
            class="absolute inset-y-0 right-5 flex items-center pointer-events-none text-[#404040]"
          >
            ▼
          </div>
        </div>
        <button
          type="button"
          onclick={getRooms}
          disabled={isLoading}
          class="w-full sm:w-[80%] md:w-[60%] py-3 rounded-xl text-xl cursor-pointer mt-5 bg-[#404040] text-white border-none hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2 disabled:opacity-70 disabled:cursor-not-allowed"
        >
          {#if isLoading}
            <span class="flex items-center justify-center">
              <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              กำลังโหลด...
            </span>
          {:else}
            ตกลง
          {/if}
        </button>
      </form>
    </div>
  {:else}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-full sm:w-[80%] md:w-[60%] lg:w-[45%] h-auto py-10 md:h-[95%] shadow-lg flex flex-col items-center justify-center p-5 overflow-y-auto"
      in:scale={{ duration: 400 }}
    >
      <h1
        class="text-[#404040] text-center font-jeju text-3xl md:text-4xl font-normal mb-6"
        in:fly={{ y: -20, duration: 400, delay: 100 }}
      >
        เลือกห้องพัก
      </h1>
      
      {#if errorMessage}
        <div 
          class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-xl mb-4 w-[90%] max-w-[550px]"
          in:fly={{ y: 20, duration: 300 }}
        >
          <p>{errorMessage}</p>
        </div>
      {/if}
      
      <div class="w-full flex flex-col items-center" in:fly={{ y: 20, duration: 400, delay: 200 }}>
        <div class="relative w-full sm:w-[80%] mb-6 transition-all duration-300 hover:scale-[1.01]">
          <select
            bind:value={selectedRoom}
            class="w-full py-3 px-5 rounded-xl border-2 border-[#404040] bg-transparent text-left appearance-none pr-10 {selectedRoom
              ? 'text-[#404040]'
              : 'text-[#8B8B8C]'} font-jeju text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          >
            <option value="" disabled selected>ห้อง</option>
            {#each rooms as room}
              <option value={room.room_number} class="text-[#404040]"
                >{room.room_number}</option
              >
            {/each}
          </select>
          <div
            class="absolute inset-y-0 right-5 flex items-center pointer-events-none text-[#404040]"
          >
            ▼
          </div>
        </div>
        
        {#if room}
          <div
            class="flex flex-col sm:flex-row justify-between gap-4 mt-2 text-center w-[90%] text-[#404040] font-jeju text-lg md:text-xl font-normal bg-white bg-opacity-50 p-4 rounded-xl"
            in:fade={{ duration: 300 }}
          >
            <p class="flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4" />
              </svg>
              ขนาด {room.size} ตรม.
            </p>
            <p class="flex items-center justify-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              {room.price} บาท/เดือน
            </p>
          </div>
          
          <div class="flex flex-col items-start gap-2.5 mt-5 w-full sm:w-[80%]" in:fade={{ duration: 300, delay: 300 }}>
            <label for="start_date" class="text-[#404040] font-jeju text-lg">วันที่เข้าอยู่</label>
            <input
              type="date"
              id="start_date"
              bind:value={start_date}
              class="w-full py-3 px-5 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all duration-300 hover:scale-[1.01]"
            />
          </div>
          
          <div class="flex flex-col items-start gap-2.5 mt-4 w-full sm:w-[80%]" in:fade={{ duration: 300, delay: 400 }}>
            <!-- svelte-ignore a11y_label_has_associated_control -->
            <label class="text-[#404040] font-jeju text-lg">ระยะเวลาของสัญญา</label>
            <div class="grid grid-cols-2 gap-4 w-full">
              <div class="flex flex-col">
                <input
                  type="number"
                  min="0"
                  max="99"
                  placeholder="ปี"
                  bind:value={contractDuration.years}
                  class="w-full py-3 px-5 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all duration-300 hover:scale-[1.01]"
                />
              </div>
              <div class="flex flex-col">
                <input
                  type="number"
                  min="0"
                  max="11"
                  placeholder="เดือน"
                  bind:value={contractDuration.months}
                  class="w-full py-3 px-5 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all duration-300 hover:scale-[1.01]"
                />
              </div>
            </div>
          </div>
          
          <div class="flex flex-col sm:flex-row gap-4 mt-8 w-full sm:w-[80%]" in:fade={{ duration: 300, delay: 500 }}>
            <button
              type="button"
              onclick={createRentalContract}
              disabled={isLoading}
              class="w-full py-3 rounded-xl text-xl cursor-pointer bg-[#404040] text-white border-none hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2 disabled:opacity-70 disabled:cursor-not-allowed"
            >
              {#if isLoading}
                <span class="flex items-center justify-center">
                  <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                  </svg>
                  กำลังสร้างสัญญา...
                </span>
              {:else}
                สร้างสัญญาเช่า
              {/if}
            </button>
            <button
              type="button"
              onclick={goBack}
              class="w-full py-3 rounded-xl text-xl cursor-pointer bg-transparent text-[#404040] border-2 border-[#404040] hover:bg-[#40404010] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040]"
            >
              ย้อนกลับ
            </button>
          </div>
        {/if}
      </div>
    </div>
  {/if}
</div>

<style>
  @font-face {
    font-family: "JejuGothic";
    src: url("/fonts/JejuGothic-Regular.ttf") format("truetype");
  }
  
  input[type="date"]::-webkit-calendar-picker-indicator {
    filter: invert(0.5);
    cursor: pointer;
  }
  
</style>
