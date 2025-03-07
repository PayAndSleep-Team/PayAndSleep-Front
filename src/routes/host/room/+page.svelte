<script>
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";
  import { fade, fly, scale } from "svelte/transition";

  let rooms = [
    {
      id: 1,
      room_number: "209-01",
      status: "available",
      size: "30",
      price: "5000",
    },
    {
      id: 2,
      room_number: "209-02",
      status: "occupied",
      size: "25",
      price: "4500",
    },
    {
      id: 3,
      room_number: "209-01",
      status: "available",
      size: "30",
      price: "5000",
    },
    {
      id: 4,
      room_number: "209-02",
      status: "occupied",
      size: "25",
      price: "4500",
    },
    {
      id: 5,
      room_number: "209-01",
      status: "available",
      size: "30",
      price: "5000",
    },
  ];

  let errorMessage = "";
  let newRoom = {
    room_number: "",
    size: "",
    price: "",
    status: "available",
  };

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

  let isModalOpen = false;
  let selectedRoom = null;
  let isRoomModalOpen = false;
  let isSubmitting = false;

  const addRoom = () => {
    isRoomModalOpen = true;
    newRoom = {
      room_number: "",
      size: "",
      price: "",
      status: "available",
    };
  };

  const closeRoomModal = (e) => {
    if (e.target === e.currentTarget) {
      isRoomModalOpen = false;
    }
  };

  const openModal = (room) => {
    selectedRoom = room;
    isModalOpen = true;
  };

  const closeModal = (e) => {
    if (e.target === e.currentTarget) {
      isModalOpen = false;
    }
  };

  const checkStatus = (status) => {
    if (status === "available") {
      return "ว่าง";
    } else if (status === "occupied") {
      return "ไม่ว่าง";
    }
  };

  function submitNewRoom() {
    if (!newRoom.room_number || !newRoom.size || !newRoom.price) {
      errorMessage = "กรุณากรอกข้อมูลให้ครบถ้วน";
      return;
    }
    isSubmitting = true;
    
    try {
      const newRoomWithId = {
        id: rooms.length + 1,
        room_number: newRoom.room_number,
        size: newRoom.size,
        price: newRoom.price,
        status: newRoom.status
      };
    
      rooms = [...rooms, newRoomWithId];
      
      isRoomModalOpen = false;
      newRoom = {
        room_number: "",
        size: "",
        price: "",
        status: "available"
      };
      showSuccessToast("เพิ่มห้องพักสำเร็จ");
    } catch (error) {
      console.error("Error creating room:", error);
      errorMessage = "ไม่สามารถเพิ่มห้องพัก";
    } finally {
      isSubmitting = false;
    }
  }

  function showSuccessToast(message) {
    const toast = document.createElement("div");
    toast.className =
      "fixed bottom-4 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg transform transition-transform duration-300 ease-in-out";
    toast.innerHTML = message;
    document.body.appendChild(toast);

    setTimeout(() => {
      toast.classList.add("translate-y-20", "opacity-0");
      setTimeout(() => {
        document.body.removeChild(toast);
      }, 300);
    }, 3000);
  }

  function gotoRentfee() {
    goto(`/host/rentfee?id=${selectedRoom.id || selectedRoom.room_number}`);
  }

  function gotoManagerooms() {
    goto(`/host/managerooms?id=${selectedRoom.id || selectedRoom.room_number}`);
  }

  function gotoVerifyPayment() {
    goto(
      `/host/verify-payment?id=${selectedRoom.id || selectedRoom.room_number}`
    );
  }
</script>

<div class="p-4 w-full min-h-screen">
  <div class="ml-3">
    <p class="font-semibold text-2xl text-white mb-4">จัดการห้องพัก</p>
  </div>

  {#if errorMessage}
    <div
      class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-xl mb-4 w-full max-w-3xl mx-auto"
      in:fade={{ duration: 300 }}
    >
      <p>{errorMessage}</p>
      <button
        class="float-right text-red-700"
        onclick={() => (errorMessage = "")}>✕</button
      >
    </div>
  {/if}

  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div
    class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-4"
  >
    <!-- create room -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105 h-20 sm:h-24"
      in:fly={{ x: -20, duration: 600, delay: 100 }}
      onclick={addRoom}
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#404040]"></div>
        <div class="px-3 text-2xl sm:text-5xl font-bold text-[#8B8B8C]">
          เพิ่มห้อง
        </div>
      </div>
    </div>

    {#each rooms as room, i (room.id || i)}
      <!-- svelte-ignore a11y_click_events_have_key_events -->
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <div
        class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105 h-20 sm:h-24"
        in:fly={{ x: -20, y: 10, duration: 400, delay: 100 + i * 100 }}
        out:fly={{ x: 20, y: 0, duration: 300 }}
        onclick={() => openModal(room)}
      >
        <div class="flex items-center h-full">
          {#if room.status === "available"}
            <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
          {:else}
            <div class="h-full w-[11px] ml-1 my-2 bg-[#D04C4C]"></div>
          {/if}
          <div class="flex justify-between w-full items-center">
            {#if room.status === "available"}
              <div class="px-3 text-2xl sm:text-5xl font-bold text-[#557B55]">
                {room.room_number}
              </div>
              <div
                class="px-3 text-xl sm:text-3xl font-semibold text-[#557B55]"
              >
                {checkStatus(room.status)}
              </div>
            {:else}
              <div class="px-3 text-2xl sm:text-5xl font-bold text-[#D04C4C]">
                {room.room_number}
              </div>
              <div
                class="px-3 text-xl sm:text-3xl font-semibold text-[#D04C4C]"
              >
                {checkStatus(room.status)}
              </div>
            {/if}
          </div>
        </div>
      </div>
    {/each}
  </div>

  {#if isModalOpen}
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <div
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
      onclick={closeModal}
      in:fade={{ duration: 200 }}
    >
      <div
        class="flex flex-col md:flex-row gap-4 p-4 max-w-full overflow-auto"
        in:scale={{ duration: 300, start: 0.9 }}
      >
        <button
          onclick={gotoManagerooms}
          class="transform transition-transform duration-200 hover:scale-105"
        >
          <div
            class="bg-[#404040] rounded-xl p-6 text-white w-64 md:w-80 h-24 flex justify-center items-center cursor-pointer shadow-lg"
          >
            <p class="text-white text-2xl font-bold text-center">
              จัดการห้องพัก
            </p>
          </div>
        </button>
        <button
          onclick={gotoVerifyPayment}
          class="transform transition-transform duration-200 hover:scale-105"
        >
          <div
            class="bg-[#404040] rounded-xl p-6 w-64 md:w-80 h-24 flex justify-center items-center cursor-pointer shadow-lg"
          >
            <p class="text-white text-2xl font-bold text-center">
              ตรวจสอบ<br />รายการชำระเงิน
            </p>
          </div>
        </button>
        <button
          onclick={gotoRentfee}
          class="transform transition-transform duration-200 hover:scale-105"
        >
          <div
            class="bg-[#404040] rounded-xl p-6 w-64 md:w-80 h-24 flex justify-center items-center cursor-pointer shadow-lg"
          >
            <p class="text-white text-2xl font-bold text-center">แจ้งค่าเช่า</p>
          </div>
        </button>
      </div>
    </div>
  {/if}

  {#if isRoomModalOpen}
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
      onclick={(e) => closeRoomModal(e)}
      in:fade={{ duration: 200 }}
    >
      <div
        class="bg-white rounded-xl p-6 w-full max-w-md mx-4 shadow-2xl"
        in:scale={{ duration: 300, start: 0.9 }}
        onclick={(e) => e.stopPropagation()}
      >
        <h2 class="text-2xl font-bold text-center mb-6 text-[#404040]">
          เพิ่มห้องพัก
        </h2>

        <form onsubmit={(e) => { e.preventDefault(); submitNewRoom(); }} class="space-y-4">
          <div>
            <label
              for="room_number"
              class="block text-sm font-medium text-gray-700 mb-1"
              >เลขห้อง</label
            >
            <input
              type="text"
              id="room_number"
              bind:value={newRoom.room_number}
              class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-[#404040] focus:border-[#404040]"
              placeholder="เช่น 101, A201"
              required
            />
          </div>

          <div>
            <label
              for="size"
              class="block text-sm font-medium text-gray-700 mb-1"
              >ขนาดห้อง (ตร.ม.)</label
            >
            <input
              type="number"
              id="size"
              bind:value={newRoom.size}
              class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-[#404040] focus:border-[#404040]"
              placeholder="เช่น 30"
              min="1"
              required
            />
          </div>

          <div>
            <label
              for="price"
              class="block text-sm font-medium text-gray-700 mb-1"
              >ราคา (บาท/เดือน)</label
            >
            <input
              type="number"
              id="price"
              bind:value={newRoom.price}
              class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-[#404040] focus:border-[#404040]"
              placeholder="เช่น 5000"
              min="1"
              required
            />
          </div>

          <div>
            <label
              for="status"
              class="block text-sm font-medium text-gray-700 mb-1"
              >สถานะห้อง</label
            >
            <select
              id="status"
              bind:value={newRoom.status}
              class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-[#404040] focus:border-[#404040]"
            >
              <option value="available">ว่าง</option>
              <option value="occupied">ไม่ว่าง</option>
            </select>
          </div>

          <div class="flex justify-end space-x-3 pt-4">
            <button
              type="button"
              class="px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-[#404040]"
              onclick={() => (isRoomModalOpen = false)}
            >
              ยกเลิก
            </button>
            <button
              type="submit"
              class="px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-[#404040] hover:bg-[#333333] focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-[#404040] disabled:opacity-50 disabled:cursor-not-allowed"
              disabled={isSubmitting}
            >
              {#if isSubmitting}
                <span class="flex items-center">
                  <svg
                    class="animate-spin -ml-1 mr-2 h-4 w-4 text-white"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                  >
                    <circle
                      class="opacity-25"
                      cx="12"
                      cy="12"
                      r="10"
                      stroke="currentColor"
                      stroke-width="4"
                    ></circle>
                    <path
                      class="opacity-75"
                      fill="currentColor"
                      d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
                    ></path>
                  </svg>
                  กำลังบันทึก...
                </span>
              {:else}
                บันทึก
              {/if}
            </button>
          </div>
        </form>
      </div>
    </div>
  {/if}
</div>
