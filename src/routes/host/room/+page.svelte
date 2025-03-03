<script>
  const rooms = [
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
</script>

<div class="p-4 w-full">
  <div class="ml-3">
    <p class="font-semibold text-2xl text-white mb-4">จัดการห้องพัก</p>
  </div>
  <div class="grid grid-cols-2 gap-4 mb-4">
    {#each rooms as room}
      <!-- svelte-ignore a11y_click_events_have_key_events -->
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-2"
        onclick={() => openModal(room)}
      >
        <div class="flex">
          {#if room.status === "ว่าง"}
            <div>
              <img src="/images/greenRect.svg" alt="" />
            </div>
          {:else if room.status === "ไม่ว่าง"}
            <div>
              <img src="/images/redRect.svg" alt="" />
            </div>
          {/if}

          <div class="flex flex-row justify-between w-full items-center px-3">
            {#if room.status === "ว่าง"}
              <div class="text-5xl font-bold text-[#557B55]">
                {room.number}
              </div>
              <div class="text-3xl font-semibold text-[#557B55]">
                {room.status}
              </div>
            {:else if room.status === "ไม่ว่าง"}
              <div class="text-5xl font-bold text-[#D04C4C]">
                {room.number}
              </div>
              <div class="text-3xl font-semibold text-[#D04C4C]">
                {room.status}
              </div>
            {/if}
          </div>
        </div>
      </div>
    {/each}
    {#if isModalOpen}
      <div
        class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
      >
        <div class="bg-[#404040] rounded-xl p-6 text-white w-80 relative">
          <button class="absolute top-2 right-2" onclick={closeModal}>
            <img src="/images/cross.svg" alt="close" class="w-6 h-6 m-2" />
          </button>
          <p class="text-2xl font-bold mb-2">{selectedRoom.number}</p>
          <p class="text-lg mb-1">ชื่อ: {selectedRoom.name}</p>
          <p class="text-lg mb-1">โทร: {selectedRoom.phoneNumber}</p>
        </div>
      </div>
    {/if}
  </div>
</div>
