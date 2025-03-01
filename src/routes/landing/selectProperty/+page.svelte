<script>
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  const Api_url = "http://localhost:3000";
  let properties = $state([]);
  let rooms = $state([]);
  let showRoomSelection = $state(false);
  let contractDuration = $state({ months: "", years: "" });
  let selectedRoom = $state("");
  let selectedProperty = $state("");
  let start_date = $state("");
  let room = $derived(rooms.find((r) => r.room_number === selectedRoom));

  onMount(async () => {
    try {
      const res = await fetch(`${Api_url}/api/get/property`, {
        method: "GET",
        credentials: "include",
      });
      properties = await res.json();
    } catch (error) {
      console.error("Error fetching properties:", error);
      alert("Failed to load properties");
    }
  });

  const getRooms = async () => {
    try {
      if (!selectedProperty) {
        alert("กรุณาเลือกที่พัก");
        return;
      }
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
      alert("Failed to load rooms");
    }
  };

  const createRentalContract = async () => {
    try {
      if (!selectedRoom) {
        alert("กรุณาเลือกห้อง");
        return;
      }
      if (!start_date) {
        alert("กรุณาเลือกวันที่เข้าอยู่");
        return;
      }
      if (!contractDuration.years) {
        alert("กรุณากรอกระยะเวลาของสัญญา");
        return;
      }
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
      alert(data.message);
      if (data.message === "สร้างสัญญาเช่าสำเร็จ") {
        goto("/tenant/waiting");
      }
    } catch (error) {
      console.error("Error creating rental contract:", error);
      alert("Failed to create rental contract");
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
    date.setMonth(date.getMonth() + Number(contractDuration.months));
    return formatDateTime(date);
  }
</script>

<div class=" w-full h-screen flex justify-end items-center pr-[5%]">
  {#if !showRoomSelection}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center p-5"
    >
      <h1
        class="text-[#404040] text-center font-jeju text-4xl font-normal mb-8"
      >
        เลือกที่พัก
      </h1>
      <form class="w-full flex flex-col items-center">
        <div class="relative w-[60%] max-w-[550px] min-w-[400px] mb-8">
          <select
            bind:value={selectedProperty}
            required
            class="w-full py-3 px-5 rounded-[25px] border-2 border-[#404040] bg-transparent text-left appearance-none pr-10 {selectedProperty
              ? 'text-[#404040]'
              : 'text-[#8B8B8C]'} font-jeju text-xl font-normal"
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
          onclick={() => {
            getRooms();
          }}
          class="w-[60%] py-3 rounded-[25px] text-xl cursor-pointer mt-5 bg-[#404040] text-white border-none"
        >
          ตกลง
        </button>
      </form>
    </div>
  {:else}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center p-5"
    >
      <h1
        class="text-[#404040] text-center font-jeju text-4xl font-normal mb-8"
      >
        เลือกห้องพัก
      </h1>
      <div class="relative w-[60%] max-w-[550px] min-w-[400px]">
        <select
          bind:value={selectedRoom}
          class="w-full py-3 px-5 rounded-[25px] border-2 border-[#404040] bg-transparent text-left appearance-none pr-10 {selectedRoom
            ? 'text-[#404040]'
            : 'text-[#8B8B8C]'} font-jeju text-xl font-normal"
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
          class="flex justify-between gap-2.5 mt-5 text-center w-[80%] text-[#8B8B8C] font-jeju text-xl font-normal"
        >
          <p>ขนาด {room.size} ตรม.</p>
          <p>บาท/เดือน {room.price} บาท</p>
        </div>
        <!-- create start date ui -->
         <div class="flex flex-col items-start gap-2.5 mt-5 text-center w-[80%] text-[#8B8B8C] font-jeju text-xl font-normal">
          <label for="start_date" class="block text-gray-700 text-xl font-bold mb-2">วันที่จะเข้าอยู่</label>
          <input type="date" id="start_date" name="start_date" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline bg-[rgba(242,242,242,0.8)]" bind:value={start_date} />
         </div>
      {/if}
      <div class="mt-5 text-center w-full">
        <p
          class="text-[#404040] font-jeju text-xl font-normal mb-3.5 pl-[10%] text-left"
        >
          ระยะเวลาของสัญญา
        </p>
        <div class="flex justify-center gap-2.5">
          <input
            type="number"
            placeholder="ปี"
            bind:value={contractDuration.years}
            class="w-[40%] py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
          <input
            type="number"
            placeholder="เดือน"
            bind:value={contractDuration.months}
            class="w-[40%] py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
      </div>
      <button
        type="button"
        onclick={createRentalContract}
        class="w-[80%] py-3 rounded-[25px] text-xl cursor-pointer mt-5 bg-[#404040] text-white border-none"
      >
        ตกลง
      </button>
    </div>
  {/if}
</div>

<style>
  @font-face {
    font-family: "JejuGothic";
    src: url("/fonts/JejuGothic-Regular.ttf") format("truetype");
  }
</style>
