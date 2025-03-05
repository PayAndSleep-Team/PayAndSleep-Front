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
    let tempName = $state("ปทิตตา ดวงแก้ว");
    let tempEmail = $state("patittadua@gmail.com");
    let tempPassword = $state("Pramandanijjanukor2546");
  
    onMount(async () => {
      try {
        const res = await fetch(`${Api_url}/api/session`, {
          method: "GET",
          credentials: "include",
        });
        user = await res.json();
        profile = user.profile;
        tempName = profile?.name;
        tempEmail = user?.email || "Enter Email";
        tempPassword = user?.password || "Enter Password";
      } catch (error) {
        console.error("Error fetching message:", error);
      }
    });
  
    function toggleEdit() {
      if (!isEditing) {
        tempName = profile?.name;
        tempEmail = user?.email;
        tempPassword = user?.password;
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
      <div class="w-full pt-16 flex flex-col items-center px-10">
        <img
          src="/images/Icon.svg"
          alt="icon"
          class="mb-6 w-24 h-24 fill-[#F2F2F2]"
        />
        {#if isEditing}
          <input class="mb-4 py-1 px-6 rounded-xl border-2  border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal" bind:value={tempName} placeholder="Enter Name"/>
        {:else}
          <p class="text-[#F2F2F2] font-normal font-jeju text-4xl">{tempName}</p>
        {/if}
  
        {#if isEditing}
          <input class="py-1 px-6 rounded-xl border-2 border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal" bind:value={tempEmail} placeholder="Enter Email"/>
        {:else}
          <p class="text-[#F2F2F2] font-normal font-jeju text-2xl mb-10">
            {tempEmail}
          </p>
        {/if}
  
        <div class="w-full max-w-2xl text-left mb-10 relative mr-24">
          <p class="text-[#F2F2F2] text-2xl font-normal leading-none mb-4">
            รหัสผ่าน
          </p>
          {#if isEditing}
            <input class="mb-4 w-full py-1 px-6 rounded-xl border-[#404040] bg-[#F2F2F2] text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal" bind:value={tempPassword} placeholder="Enter Password"/>
          {:else}
            <p class="text-[#F2F2F2] text-xl mb-4 pl-10">{tempPassword}</p>
          {/if}
          <div
            class="absolute bottom-0 left-0 w-full border-t-2"
          ></div>
        </div>
        <div class="w-full max-w-3xl">
          <p class="text-[#F2F2F2] text-2xl font-normal leading-none mb-6">
            ประวัติการเช่า
          </p>
          <div class="grid grid-cols-2 gap-6">
            {#each history as item}
            <div class="bg-white rounded-xl shadow overflow-hidden p-2">
              <div class="flex">
                <div><img src="/images/grayRect.svg" alt="" class="h-20"/></div>
                <div class="flex flex-col justify-center">
                  <div class="px-3 text-xl text-[#404040]">{item.month} ยอดรวม {item.amount}</div>
                  <div
                    class="px-3 text-xl text-[#404040]"
                  >
                    สภานะ: {item.status}
                  </div>
                </div>
              </div>
            </div>
            {/each}
          </div>
        </div>
      </div>
    </div>
    <button onclick={toggleEdit} class="absolute bottom-12 right-12 border-none p-0">
      {#if isEditing}
          <div class="w-12 h-12 flex items-center justify-center">
              <img src="/images/confirm.svg" alt="Confirm" class="w-8 h-8" />
          </div>
      {:else}
          <div class="w-12 h-12 bg-[url('/images/editbg.svg')] bg-cover flex items-center justify-center">
              <img src="/images/edit.svg" alt="Edit" class="w-8 h-8" />
          </div>
      {/if}
  </button>
  
  </div>