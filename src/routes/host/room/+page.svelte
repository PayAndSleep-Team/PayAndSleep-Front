<script>
  import { onMount } from "svelte";

  const Api_url = "http://localhost:3000";

  onMount(async () => {
    try {
      const res = await fetch(`${Api_url}/api/get/rooms`, {
        method: "GET",
        credentials: "include",
      });
      rooms = await res.json();
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });

  let rooms = [
    {
      number: "409-01",
      status: "ว่าง",
      name: "John Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-02",
      status: "ไม่ว่าง",
      name: "Jane Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-03",
      status: "ไม่ว่าง",
      name: "John Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-04",
      status: "ว่าง",
      name: "Jane Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-05",
      status: "ไม่ว่าง",
      name: "John Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-06",
      status: "ว่าง",
      name: "Jane Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-07",
      status: "ว่าง",
      name: "John Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-08",
      status: "ไม่ว่าง",
      name: "Jane Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-09",
      status: "ว่าง",
      name: "John Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-10",
      status: "ว่าง",
      name: "Jane Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-11",
      status: "ว่าง",
      name: "John Doe",
      phoneNumber: "081-234-5678",
    },
    {
      number: "409-12",
      status: "ว่าง",
      name: "Jane Doe",
      phoneNumber: "081-234-5678",
    },
  ];
  let isModalOpen = false;
  let selectedRoom = null;

  const openModal = (room) => {
    selectedRoom = room;
    isModalOpen = true;
  };

  const closeModal = () => {
    isModalOpen = false;
  };

  const checkStatus = (status) => {
    if (status === "available") {
      return "ว่าง";
    } else if (status === "occupied") {
      return "ไม่ว่าง";
    }
  };
</script>

<div class="p-4 w-full">
  <div class="ml-3">
    <p class="font-semibold text-2xl text-white mb-4">จัดการห้องพัก</p>
  </div>
  <div class="grid grid-cols-2 gap-4 mb-4">
    <!-- create room -->
    {#each rooms as room}
      <!-- svelte-ignore a11y_click_events_have_key_events -->
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-2 cursor-pointer"
        onclick={() => openModal(room)}
      >
        <div class="flex">
          {#if room.status === "available"}
            <div>
              <img src="/images/greenRect.svg" alt="" />
            </div>
          {:else if room.status === "occupied"}
            <div>
              <img src="/images/redRect.svg" alt="" />
            </div>
          {/if}

          <div class="flex flex-row justify-between w-full items-center px-3">
            {#if room.status === "available"}
              <div class="text-5xl font-bold text-[#557B55]">
                {room.room_number}
              </div>
              <div class="text-3xl font-semibold text-[#557B55]">
                {checkStatus(room.status)}
              </div>
            {:else if room.status === "occupied"}
              <div class="text-5xl font-bold text-[#D04C4C]">
                {room.room_number}
              </div>
              <div class="text-3xl font-semibold text-[#D04C4C]">
                {checkStatus(room.status)}
              </div>
            {/if}
          </div>
        </div>
      </div>
    {/each}
    {#if isModalOpen}
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <!-- svelte-ignore a11y_click_events_have_key_events -->
      <div
        class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 gap-6"
        onclick={closeModal}
      >
        <div
          class="bg-[#404040] rounded-xl p-6 text-white w-80 h-24 flex justify-center items-center cursor-pointer"
        >
          <a href="/" class="text-white text-2xl font-bold text-center"
            >จัดการห้องพัก</a
          >
        </div>
        <div
          class="bg-[#404040] rounded-xl p-6 w-80 h-24 flex justify-center items-center cursor-pointer"
        >
          <a href="/" class="text-white text-2xl font-bold text-center">
            ตรวจสอบ<br />รายการชำระเงิน
          </a>
        </div>
        <div
          class="bg-[#404040] rounded-xl p-6 w-80 h-24 flex justify-center items-center cursor-pointer"
        >
          <a href="/" class="text-white text-2xl font-bold text-center">
            แจ้งค่าเช่า
          </a>
        </div>
      </div>
    {/if}
  </div>
</div>
