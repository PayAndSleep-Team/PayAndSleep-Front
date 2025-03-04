<script>
    import { onMount } from "svelte";
  
    const Api_url = "http://localhost:3000";
    let user = $state({});
    let profile = $state({});
    let history = [
      { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
      { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
      { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
      { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
      { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
      { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
    ];
  
    let isEditing = $state(false);
    let tempName = $state("temp");
    let tempEmail = $state("temp");
    let tempPassword = $state("temp");
  
    onMount(async () => {
      try {
        const res = await fetch(`${Api_url}/api/session`, {
          method: "GET",
          credentials: "include",
        });
        user = await res.json();
        profile = user.profile;
        tempName = profile?.name || "temp";
        tempEmail = user?.email || "temp";
        tempPassword = user?.password || "temp";
      } catch (error) {
        console.error("Error fetching message:", error);
      }
    });
  
    function toggleEdit() {
      if (!isEditing) {
        tempName = profile?.name || "temp";
        tempEmail = user?.email || "temp";
        tempPassword = user?.password || "temp";
      } else {
        if (profile) {
          profile.name = tempName;
        }
        if (user) {
          user.email = tempEmail;
          user.password = tempPassword;
        }
      }
      isEditing = !isEditing;
    }
  </script>
  
  <div class="w-full min-h-full relative">
    <div class="w-full h-full flex flex-col items-center justify-center">
      <div class="w-[85%] h-[90%] pt-16 flex flex-col items-center px-10">
        <img
          src="/images/Icon.svg"
          alt="icon"
          class="mb-6 w-24 h-24 fill-[#F2F2F2]"
        />
        {#if isEditing}
          <input class="mb-4 w-[10%] py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal" bind:value={tempName} />
        {:else}
          <p class="text-[#F2F2F2] font-normal font-jeju text-4xl">{tempName}</p>
        {/if}
  
        {#if isEditing}
          <input class="w-[10%] py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal" bind:value={tempEmail} />
        {:else}
          <p class="text-[#F2F2F2] font-normal font-jeju text-2xl mb-10">
            {tempEmail}
          </p>
        {/if}
  
        <div class="w-full max-w-2xl text-left mb-10 relative">
          <p class="text-[#F2F2F2] text-2xl font-normal leading-none mb-4">
            รหัสผ่าน
          </p>
          {#if isEditing}
            <input class="mb-4 w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal" bind:value={tempPassword} />
          {:else}
            <p class="text-[#F2F2F2] text-xl mb-4 pl-10">{tempPassword}</p>
          {/if}
          <div
            class="absolute bottom-0 left-0 w-full border-t border-gray-400"
          ></div>
        </div>
        <div class="w-full max-w-3xl">
          <p class="text-[#F2F2F2] text-2xl font-normal leading-none mb-10">
            ประวัติการเช่า
          </p>
          <div class="grid grid-cols-2 gap-6">
            {#each history as item}
              <div
                class="bg-gray-200 border-[#404040] shadow-md rounded-lg p-4 border-l-4 pl-4"
              >
                <p class="text-black text-xl font-medium">
                  {item.month} ยอดรวม {item.amount}
                </p>
                <p class="text-gray-600 text-lg">
                  สถานะ: {item.status}
                </p>
              </div>
            {/each}
          </div>
        </div>
      </div>
    </div>
      <button
        onclick={toggleEdit}
        class="absolute bottom-4 right-4 bg-transparent border-none p-0"
      >
          {#if isEditing}
              <img src="/images/confirm.svg" alt="Confirm" class="w-8 h-8" />
          {:else}
              <img src="/images/edit.svg" alt="Edit" class="w-8 h-8" />
          {/if}
      </button>
  </div>