<script>
  import { onMount, onDestroy } from "svelte";
  import { page } from "$app/stores";
  import { fade, fly, scale } from "svelte/transition";

  const Api_url = "http://localhost:3000";

  let tempRoom = "45/96";
  let tempBuilding = "ตึก A";
  let tempMessage = "จะมีการปรับปรุงพื้นสระว่ายนํ้า ในวันที่ 17/2/67";
  let tempRequest = "ไม่พบคำร้องซ่อมบำรุง";
  let notifications = [
    {
      id: 1,
      message: "มีการปรับปรุงพื้นสระว่ายน้ำ ในวันที่ 17/2/67",
      status: "unread",
    },
  ];
  let maintenanceRequest = [
    {
      id: 1,
      description: "โดยไฟห้องไม่ติดตรงชุดหลอดบนหน้าต่าง",
      status: "ดำเนินการ",
    },
  ];
  function markAsRead(id) {
    notifications = notifications.map((notification) =>
      notification.id === id
        ? { ...notification, status: "read" }
        : notification
    );
  }
  onMount(async () => {
    const unsubscribe = page.subscribe(($page) => {});

    onDestroy(unsubscribe);
  });
</script>

<div
  class="w-full relative flex flex-col items-center px-4 md:px-6 lg:px-8"
  in:fade={{ duration: 300 }}
>
  <div class="relative w-full">
    <img
      src="/images/mrbg.svg"
      class="w-full rounded-lg object-cover"
      alt="roompic"
    />
    <div class="ml-4 sm:ml-6 absolute bottom-4 left-2 sm:left-6 text-white">
      <p class="text-2xl sm:text-3xl md:text-4xl font-bold pb-1 sm:pb-2">{tempRoom}</p>
      <p class="text-lg sm:text-xl font-medium">{tempBuilding}</p>
    </div>
  </div>

  <div class="w-full flex flex-col mt-4 sm:mt-6 md:mt-8">
    {#if notifications.length > 0}
      <div class="space-y-2 sm:space-y-3">
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <!-- svelte-ignore a11y_click_events_have_key_events -->
        {#each notifications as notification, i}
          <div
            in:fly={{ y: 20, duration: 300, delay: i * 100 }}
            class="flex items-center bg-[#F2F2F2] rounded-lg shadow-md p-3 sm:p-4
                               transform transition-all duration-300 hover:scale-[1.02] hover:shadow-lg
                               cursor-pointer"
            on:click={() => markAsRead(notification.id)}
          >
            {#if notification.status === "read"}
              <img
                src="/images/bell.svg"
                alt="bell"
                class="w-6 h-6 sm:w-8 sm:h-8 md:w-10 md:h-10 mr-2 sm:mr-3 md:mr-4 transition-transform hover:rotate-12"
              />
            {:else}
              <div class="relative">
                <img
                  src="/images/redBell.svg"
                  alt="bell"
                  class="w-6 h-6 sm:w-8 sm:h-8 md:w-10 md:h-10 mr-2 sm:mr-3 md:mr-4 transition-transform hover:rotate-12"
                />
                <span
                  class="absolute top-0 right-0 h-2 w-2 sm:h-3 sm:w-3 bg-red-500 rounded-full"
                ></span>
              </div>
            {/if}
            <p class="text-sm sm:text-base md:text-lg text-[#404040] flex-1">
              {notification.message}
            </p>
          </div>
        {/each}
      </div>
    {:else}
      <div class="bg-[#F2F2F2] text-[#8B8B8C] p-4 rounded-lg text-center">
        ไม่มีการแจ้งเตือนในขณะนี้
      </div>
    {/if}

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3 sm:gap-4 md:gap-6 mt-6 sm:mt-8">
      <!-- Maintenance Request Card -->
      {#each maintenanceRequest as req, i}
        <div
          class="bg-[#F2F2F2] text-black p-3 sm:p-4 rounded-2xl shadow flex flex-col justify-between transform transition-all duration-300 hover:shadow-lg hover:-translate-y-1 h-[180px] sm:h-[200px]"
          in:fly={{ y: 20, duration: 300, delay: 150 + i * 50 }}
          out:fly={{ y: -20, duration: 300 }}
        >
            <p class="text-xl sm:text-2xl font-bold text-center">คำร้องซ่อมบำรุง {i + 1}</p>
          <div class="flex justify-center">
            <p
              class="text-xs sm:text-sm text-[#8B8B8C] my-2 sm:my-3 mt-3 sm:mt-5 text-center line-clamp-2"
            >
              {req.description}
            </p>
          </div>
          {#if req.status === "ดำเนินการ"}
            <button
              class="px-4 sm:px-6 py-1.5 sm:py-2 rounded-xl text-xs sm:text-sm mx-auto bg-[#557B55] text-white hover:bg-[#446644] transition-colors"
            >
              {req.status}
            </button>
          {:else if req.status === "เสร็จสิ้น"}
            <button
              class="px-4 sm:px-6 py-1.5 sm:py-2 rounded-xl text-xs sm:text-sm mx-auto bg-[#546882] text-white hover:bg-[#435771] transition-colors"
            >
              {req.status}
            </button>
          {/if}
        </div>
      {/each}
    </div>
  </div>
</div>
