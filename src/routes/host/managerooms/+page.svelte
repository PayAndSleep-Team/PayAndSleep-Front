<script>
  import { onMount, onDestroy } from "svelte";
  import { page } from "$app/stores";

  const Api_url = "http://localhost:3000";

  let isEditing = false;
  let tempRoom = "409-01";
  let tempSize = "459 x 4645";
  let tempPrice = "6500";
  let tempAvailable = "ว่าง";

  function toggleEdit() {
    isEditing = !isEditing;
  }

  async function save() {
    try {
      const res = await fetch(`${Api_url}/api/update/room`, {
        method: "PUT",
        credentials: "include",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          id: room_id,
          size: tempSize,
          price: tempPrice,
          status: tempAvailable === "ว่าง" ? "available" : "unavailable",
        }),
      });
      const data = await res.json();
      if (data.message === "อัพเดทห้องสำเร็จ") {
        toggleEdit();
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  }

  onMount(async () => {
    const unsubscribe = page.subscribe(($page) => {
      room_id = $page.url.searchParams.get("id") || "";
    });

    onDestroy(unsubscribe);
    try {
      const res = await fetch(`${Api_url}/api/get/room?room_id=${room_id}`, {
        method: "GET",
        credentials: "include",
      });
      const data = await res.json();
      tempRoom = data.room_number;
      tempSize = data.size;
      tempPrice = data.price;
      tempAvailable = data.status;
      if (tempAvailable === "available") {
        tempAvailable = "ว่าง";
      } else {
        tempAvailable = "ไม่ว่าง";
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });

  let room_id = "";
</script>

<div class="w-full relative flex flex-col items-center pr-4">
  <div class="relative w-full">
    <img src="/images/mrbg.svg" class="w-full rounded-lg" alt="roompic" />
    <div class="ml-6 absolute bottom-4 left-6 text-[#F2F2F2]">
      <p class="text-xl font-medium">จัดการห้องพัก</p>
      <p class="text-4xl font-bold">{tempRoom}</p>
    </div>
  </div>

  <div class="ml-32 mt-8 text-[#F2F2F2] flex flex-col h-full w-full">
    <div class="mb-6">
      <p class="mb-2 text-[#F2F2F2] font-normal font-jeju text-xl">ขนาด</p>
      {#if isEditing}
        <input
          class="px-6 mb-2 rounded-xl border border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] text-[#404040] w-[30%] font-jeju text-lg font-normal"
          bind:value={tempSize}
          placeholder="ขนาดห้อง"
        />
      {:else}
        <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">
          {tempSize} ตารางเมตร
        </p>
      {/if}
      <div class="w-[80%] border-t border-[#A4A5A6] mt-2"></div>
    </div>

    <div class="mb-6">
      <p class="mb-2 text-[#F2F2F2] font-normal font-jeju text-xl">ราคา</p>
      {#if isEditing}
        <input
          class="px-6 mb-2 rounded-xl border border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] text-[#404040] w-[30%] font-jeju text-lg font-normal"
          bind:value={tempPrice}
          placeholder="ราคาห้องต่อเดือน"
          type="number"
        />
      {:else}
        <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">
          {tempPrice} บาท/เดือน
        </p>
      {/if}
      <div class="w-[80%] border-t border-[#A4A5A6] mt-2"></div>
    </div>

    <div class="mb-6">
      <p class="mb-2 text-[#F2F2F2] font-normal font-jeju text-xl">สถานะ</p>
      {#if isEditing}
        <select
          class="px-6 mb-2 rounded-xl border border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] text-[#404040] w-[30%] font-jeju text-lg font-normal"
          bind:value={tempAvailable}
        >
          <option value="ว่าง">ว่าง</option>
          <option value="ไม่ว่าง">ไม่ว่าง</option>
        </select>
      {:else}
        <p class="ml-6 mb-4 text-[#F2F2F2] font-normal font-jeju text-xl">
          <span
            class={tempAvailable === "ว่าง" ? "text-green-400" : "text-red-400"}
          >
            {tempAvailable}
          </span>
        </p>
      {/if}
      <div class="w-[80%] border-t border-[#A4A5A6] mt-2"></div>
    </div>
  </div>
</div>
<!-- svelte-ignore a11y_no_static_element_interactions -->
<!-- svelte-ignore a11y_click_events_have_key_events -->
{#if isEditing}
  <button
    onclick={toggleEdit}
    class="fixed bottom-8 right-8 md:bottom-12 md:right-12 border-none p-0 transform transition-all duration-300 hover:scale-110"
    in:fade={{ duration: 300, delay: 600 }}
  >
    <div
      class="w-12 h-12 bg-[#404040] rounded-full flex items-center justify-center"
      onclick={save}
    >
      <img src="/images/confirm.svg" alt="Confirm" class="w-8 h-8" />
    </div>
  </button>
{:else}
  <button
    onclick={toggleEdit}
    class="fixed bottom-8 right-8 md:bottom-12 md:right-12 border-none p-0 transform transition-all duration-300 hover:scale-110"
    in:fade={{ duration: 300, delay: 600 }}
  >
    <div
      class="w-12 h-12 bg-[url('/images/editbg.svg')] bg-cover flex items-center justify-center"
    >
      <img src="/images/edit.svg" alt="Edit" class="p-1 w-8 h-8" />
    </div>
  </button>
{/if}
