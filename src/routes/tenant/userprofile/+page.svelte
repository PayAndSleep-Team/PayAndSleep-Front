<script>
  import { fade, fly, scale } from "svelte/transition";
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

  async function save() {
    try {
      const res = await fetch(`${Api_url}/api/update/profile`, {
        method: "PUT",
        credentials: "include",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          name: tempName,
          email: tempEmail,
          password: tempPassword,
        }),
      });
      const data = await res.json();
      if (data.message === "อัพเดทโปรไฟล์สำเร็จ") {
        toggleEdit();
      }
    } catch (error) {
      console.error("Error fetching message:", error);
    }
  }

  async function logout() {
    try {
      const res = await fetch(`${Api_url}/api/logout`, {
        method: "GET",
        credentials: "include",
      });
      const data = await res.json();
      alert(data.message);
      if (data.message === "ออกจากระบบสำเร็จ") {
        window.location.href = "/landing/login";
      }
    } catch (error) {
      console.error("Error fetching message:", error);
    }
  }
</script>

<div class="w-full min-h-full relative p-4 md:p-8" in:fade={{ duration: 300 }}>
  <div class="w-full h-full flex flex-col items-center justify-center">
    <div
      class="w-full max-w-4xl pt-8 md:pt-16 flex flex-col items-center px-4 md:px-10"
    >
      <!-- Profile Image -->
      <div class="relative mb-6" in:scale={{ duration: 400, delay: 100 }}>
        <img
          src="/images/Icon.svg"
          alt="icon"
          class="w-20 h-20 md:w-24 md:h-24 fill-[#F2F2F2] transition-transform duration-300 hover:scale-105"
        />
      </div>

      <!-- Profile Info -->
      <div
        class="w-full flex flex-col items-center space-y-4"
        in:fly={{ y: 20, duration: 400, delay: 200 }}
      >
        {#if isEditing}
          <div class="w-full max-w-md relative mb-4">
            <label for="name" class="text-[#F2F2F2] text-xl absolute -top-6 left-2">ชื่อ-นามสกุล</label>
            <input
              id="name"
              class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#F2F2F2]
                               text-center placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
              bind:value={tempName}
              placeholder="กรอกชื่อ"
            />
          </div>
        {:else}
          <p
            class="text-[#F2F2F2] font-normal font-jeju text-3xl md:text-4xl text-center"
          >
            {tempName}
          </p>
        {/if}

        {#if isEditing}
          <div class="w-full max-w-md relative">
            <label for="email" class="text-[#F2F2F2] text-xl absolute -top-6 left-2">อีเมล</label>
            <input
              id="email"
              class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#F2F2F2]
                               text-center placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
              bind:value={tempEmail}
              placeholder="กรอกอีเมล"
              type="email"
            />
          </div>
        {:else}
          <p
            class="text-[#F2F2F2] font-normal font-jeju text-xl md:text-2xl mb-6 text-center"
          >
            {tempEmail}
          </p>
        {/if}
      </div>

      <!-- Password Section -->
      <div
        class="w-full max-w-2xl text-left mb-10 relative px-4 md:px-0"
        in:fly={{ y: 20, duration: 400, delay: 300 }}
      >
        <p
          class="text-[#F2F2F2] text-xl md:text-2xl font-normal leading-none mb-4"
        >
          รหัสผ่าน
        </p>
        {#if isEditing}
          <div class="relative">
            <input
              class="w-full py-2 px-6 border-2 border-[#404040] text-[#F2F2F2]
                               text-left placeholder-[#8B8B8C] rounded-xl font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
              bind:value={tempPassword}
              placeholder="กรอกรหัสผ่าน"
              id="password"
            />
            <!-- svelte-ignore a11y_consider_explicit_label -->
            <button 
              class="absolute right-4 top-1/2 transform -translate-y-1/2 text-[#F2F2F2] opacity-70 hover:opacity-100"
            >
            </button>
          </div>
        {:else}
          <p class="text-[#F2F2F2] text-lg md:text-xl mb-4 pl-10">
            {tempPassword}
          </p>
        {/if}
        <div
          class="absolute bottom-0 left-0 w-full border-t-2 border-white border-opacity-20"
        ></div>
      </div>

      <div
        class="w-full max-w-2xl text-left mb-10 relative px-4 md:px-0"
        in:fly={{ y: 20, duration: 400, delay: 300 }}
      >
        <p
          class="text-[#F2F2F2] text-xl md:text-2xl font-normal leading-none mb-4"
        >
          เบอร์โทรศัพท์
        </p>
        {#if isEditing}
          <input
            class="w-full py-2 px-6 rounded-xl border-2 border-[#404040] text-[#F2F2F2]
                             text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal
                             focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
            bind:value={tempPassword}
            placeholder="กรอกเบอร์โทรศัพท์"
            type="tel"
            pattern="[0-9]{10}"
          />
        {:else}
          <p class="text-[#F2F2F2] text-lg md:text-xl mb-4 pl-10">
            {tempPassword}
          </p>
        {/if}
        <div
          class="absolute bottom-0 left-0 w-full border-t-2 border-white border-opacity-20"
        ></div>
      </div>

      <!-- Rental History -->
      <div
        class="w-full max-w-3xl px-4 md:px-0"
        in:fly={{ y: 20, duration: 400, delay: 400 }}
      >
        <p
          class="text-[#F2F2F2] text-xl md:text-2xl font-normal leading-none mb-6"
        >
          ประวัติการเช่า
        </p>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 md:gap-6">
          {#each history as item, i}
            <div
              class="rounded-xl max-h-28 overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
              in:fly={{ y: 20, duration: 300, delay: 500 + i * 100 }}
            >
              <div class="flex items-center h-full">
                <div class="h-full w-[11px] ml-1 my-2 bg-[#F2F2F2]"></div>
                <div class="flex flex-col justify-center flex-1 ml-3">
                  <div class="flex flex-row">
                    <div class="text-base md:text-xl text-[#F2F2F2]">
                      {item.month}
                    </div>
                    <div class="text-base md:text-xl text-[#F2F2F2]">
                      &nbsp;&nbsp;ยอดรวม {item.amount}
                    </div>
                  </div>
                  <div class="text-base md:text-xl text-[#F2F2F2]">
                    สถานะ: {item.status}
                  </div>
                </div>
              </div>
            </div>
          {/each}
        </div>

        <!-- Logout Button -->
        <div
          class="mt-10 flex justify-center"
          in:fly={{ y: 20, duration: 300, delay: 800 }}
        >
          <button
            class="bg-white bg-opacity-10 hover:bg-opacity-20 text-white py-2 px-8 rounded-xl
                               border border-white border-opacity-30 transition-all duration-300 hover:scale-105
                               flex items-center space-x-2"
            onclick={() => logout()}
          >
            <span>ออกจากระบบ</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</div>
<!-- Edit Button -->
{#if isEditing}
  <button
    onclick={save}
    class="fixed bottom-8 right-8 md:bottom-12 md:right-12 border-none p-0 transform transition-all duration-300 hover:scale-110"
    in:fade={{ duration: 300, delay: 100 }}
  >
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div
      class="w-12 h-12 bg-[#404040] rounded-full flex items-center justify-center"
    >
      <img src="/images/confirm.svg" alt="Confirm" class="w-8 h-8" />
    </div>
  </button>
{:else}
  <button
    onclick={toggleEdit}
    class="fixed bottom-8 right-8 md:bottom-12 md:right-12 border-none p-0 transform transition-all duration-300 hover:scale-110"
    in:fade={{ duration: 300, delay: 100 }}
  >
    <div
      class="w-12 h-12 bg-[url('/images/editbg.svg')] bg-cover flex items-center justify-center"
    >
      <img src="/images/edit.svg" alt="Edit" class="p-1 w-8 h-8" />
    </div>
  </button>
{/if}