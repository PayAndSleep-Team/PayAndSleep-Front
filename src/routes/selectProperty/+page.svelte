<script>
    import { goto } from "$app/navigation";
  
    const Api_url = "http://localhost:3000";
    let properties = ["Option 1", "Option 2", "Option 3"];
    let showRoomSelection = false;
    let contractDuration = { months: "", years: "" };
    let selectedRoom = "101";
    let selectedProperty = "";
  
    function handleRoomSelection() {
      if (contractDuration.months && contractDuration.years && selectedRoom) {
        alert(`Room ${selectedRoom} selected for ${contractDuration.years} years and ${contractDuration.months} months.`);
      } else {
        alert("Please select a room and enter the contract duration.");
      }
    }
  
    function handlePropertySelect(event) {
      selectedProperty = event.target.value;
    }
  </script>
  
  <div class="bg-cover bg-center bg-no-repeat w-screen h-screen flex justify-end items-center pr-[5%]" style="background-image: url('/images/bg.png');">
    {#if !showRoomSelection}
      <div class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center p-5">
        <h1 class="text-[#404040] text-center font-jeju text-4xl font-normal mb-8">เลือกที่พัก</h1>
        <form class="w-full flex flex-col items-center">
          <div class="relative w-[60%] max-w-[550px] min-w-[400px] mb-8">
            <select
              bind:value={selectedProperty}
              required
              on:change={handlePropertySelect}
              class="w-full py-3 px-5 rounded-[25px] border-2 border-[#404040] bg-transparent text-lg text-left appearance-none pr-10 text-[#8B8B8C] font-jeju text-xl font-normal"
            >
              <option value="" disabled selected>ที่พัก</option>
              {#each properties as property}
                <option value={property} class="text-[#404040]">{property}</option>
              {/each}
            </select>
            <div class="absolute inset-y-0 right-5 flex items-center pointer-events-none text-[#404040]">▼</div>
          </div>
          <button
            type="button"
            on:click={() => showRoomSelection = true}
            class="w-[60%] py-3 rounded-[25px] text-xl cursor-pointer mt-5 bg-[#404040] text-white border-none"
          >
            ตกลง
          </button>
        </form>
      </div>
    {:else}
      <div class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center p-5">
        <h1 class="text-[#404040] text-center font-jeju text-4xl font-normal mb-8">เลือกห้องพัก</h1>
        <div class="relative w-[60%] max-w-[550px] min-w-[400px] mb-8">
          <select
            bind:value={selectedRoom}
            class="w-full py-3 px-5 rounded-[25px] border-2 border-[#404040] bg-transparent text-lg text-left appearance-none pr-10 text-[#8B8B8C] font-jeju text-xl font-normal"
          >
            <option value="101">ห้อง 101</option>
            <option value="102">ห้อง 102</option>
            <option value="103">ห้อง 103</option>
          </select>
          <div class="absolute inset-y-0 right-5 flex items-center pointer-events-none text-[#404040]">▼</div>
        </div>
        <div class="grid grid-cols-2 grid-rows-2 gap-2.5 mt-5 text-center w-[50%] text-[#8B8B8C] font-jeju text-xl font-normal">
          <p>ขนาด</p>
          <p>บาท/เดือน</p>
          <p>ทีวี ตู้เย็น เตียง</p>
        </div>
        <div class="mt-5 text-center w-full">
          <p class="text-[#404040] font-jeju text-xl font-normal mb-3.5 pl-[20%] text-left">ระยะเวลาของสัญญา</p>
          <div class="flex justify-center gap-2.5">
            <input
              type="number"
              placeholder="เดือน"
              bind:value={contractDuration.months}
              class="w-[30%] py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-lg text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
            />
            <input
              type="number"
              placeholder="ปี"
              bind:value={contractDuration.years}
              class="w-[30%] py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-lg text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
            />
          </div>
        </div>
        <button
          type="button"
          on:click={handleRoomSelection}
          class="w-[60%] py-3 rounded-[25px] text-xl cursor-pointer mt-5 bg-[#404040] text-white border-none"
        >
          ตกลง
        </button>
      </div>
    {/if}
  </div>
  
  <style>
    @font-face {
      font-family: 'JejuGothic';
      src: url('/fonts/JejuGothic-Regular.ttf') format('truetype');
    }
  </style>