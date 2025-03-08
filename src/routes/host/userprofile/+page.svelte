<script>
    import { fade, fly, scale } from "svelte/transition";
    import { onMount } from "svelte";
  
    const Api_url = "http://localhost:3000";
    let user = $state({});
    let profile = $state({});
    let property = $state({});
  
    let isEditing = $state(false);
    let tempName = $state("ปทิตตา ดวงแก้ว");
    let tempEmail = $state("patittadua@gmail.com");
    let tempPassword = $state("Pramandanijjanukor2546");
    let tempPhone = $state("0812345678");
    let tempBank = $state("กรุงเทพ");
    let tempAccount = $state("123-4-56789-0");
    let tempAddress = $state("123 หมู่ 4 ต.หนองหาร อ.เมือง จ.เชียงใหม่ 50000");
  
    onMount(async () => {
      try {
        const res = await fetch(`${Api_url}/api/session`, {
          method: "GET",
          credentials: "include",
        });
        user = await res.json();
        profile = user.profile;
        property = user.property;
        tempName = profile?.name;
        tempEmail = user?.email || "Enter Email";
        tempPassword = user?.password || "Enter Password";
        tempPhone = profile?.phone || "Enter Phone";
        tempBank = property?.bank_name || "Enter Bank";
        tempAccount = property?.bank_account_number || "Enter Account";
        tempAddress = property?.address || "Enter Address";
      } catch (error) {
        console.error("Error fetching message:", error);
      }
    });
  
    function toggleEdit() {
      if (!isEditing) {
        tempName = profile?.name;
        tempEmail = user?.email;
        tempPassword = user?.password;
        tempPhone = profile?.phone;
        tempBank = property?.bank_name;
        tempAccount = property?.bank_account_number;
        tempAddress = property?.address;
      } else {
        if (profile) {
          profile.name = tempName;
          profile.phone = tempPhone;
        }
        if (user) {
          user.email = tempEmail;
          user.password = tempPassword;
        }
        if (property) {
          property.bank_name = tempBank;
          property.bank_account_number = tempAccount;
          property.address = tempAddress;
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
            phone: tempPhone,
            password: tempPassword,
          }),
        });
        const data = await res.json();
        const res2 = await fetch(`${Api_url}/api/update/property`, {
          method: "PUT",
          credentials: "include",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            bank_name: tempBank,
            bank_account_number: tempAccount,
            address: tempAddress,
          }),
        });
        const data2 = await res2.json();
        if (data.message === "อัพเดทโปรไฟล์สำเร็จ" && data2.message === "อัพเดท Property สำเร็จ") {
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
      <div class="w-full max-w-4xl pt-8 md:pt-16 flex flex-col items-center px-4 md:px-10">
        <!-- Profile Image -->
        <div class="relative mb-6" in:scale={{ duration: 400, delay: 100 }}>
          <img
            src="/images/Icon.svg"
            alt="icon"
            class="w-20 h-20 md:w-24 md:h-24 fill-[#F2F2F2] transition-transform duration-300 hover:scale-105"
          />
        </div>
  
        <!-- Profile Info -->
        <div class="w-full flex flex-col items-center space-y-4" in:fly={{ y: 20, duration: 400, delay: 200 }}>
          {#if isEditing}
            <div class="w-full max-w-md relative mb-4">
              <label for="name" class="text-[#F2F2F2] text-xl absolute -top-6 left-2">ชื่อ-นามสกุล</label>
              <input
                id="name"
                class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80 font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
                bind:value={tempName}
                placeholder="กรอกชื่อ"
              />
            </div>
          {:else}
            <p class="text-[#F2F2F2] font-normal font-jeju text-3xl md:text-4xl text-center">
              {tempName}
            </p>
          {/if}

          {#if isEditing}
            <div class="w-full max-w-md relative">
              <label for="email" class="text-[#F2F2F2] text-xl absolute -top-6 left-2">อีเมล</label>
              <input
                id="email"
                class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80 font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
                bind:value={tempEmail}
                placeholder="กรอกอีเมล"
                type="email"
              />
            </div>
          {:else}
            <p class="text-[#F2F2F2] font-normal font-jeju text-xl md:text-2xl mb-6 text-center">
              {tempEmail}
            </p>
          {/if}
        </div>

        <!-- Phone Section -->
        <div class="w-full max-w-2xl text-left mb-4 mt-4 relative px-4 md:px-0" in:fly={{ y: 20, duration: 400, delay: 300 }}>
          <p class="text-[#F2F2F2] text-lg md:text-xl font-normal leading-none mb-4">
            เบอร์โทรศัพท์
          </p>
          {#if isEditing}
            <div class="relative">
              <input
                class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80 font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
                bind:value={tempPhone}
                placeholder="กรอกเบอร์โทรศัพท์"
                type="tel"
                pattern="[0-9]{10}"
              />
            </div>
          {:else}
            <p class="text-[#F2F2F2] text-md md:text-lg mb-4 pl-10">
              {tempPhone}
            </p>
          {/if}
          <div class="absolute bottom-0 left-0 w-full border-t-2 border-white border-opacity-20"></div>
        </div>

        <!-- Password Section -->
        <div class="w-full max-w-2xl text-left mb-4 relative px-4 md:px-0" in:fly={{ y: 20, duration: 400, delay: 300 }}>
          <p class="text-[#F2F2F2] text-lg md:text-xl font-normal leading-none mb-4">
            รหัสผ่าน
          </p>
          {#if isEditing}
            <div class="relative">
              <input
                id="password"
                class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80 font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
                bind:value={tempPassword}
                placeholder="กรอกรหัสผ่าน"
              />
              <!-- svelte-ignore a11y_consider_explicit_label -->
              <button 
                type="button"
                class="absolute right-4 top-1/2 transform -translate-y-1/2 text-[#F2F2F2] opacity-70 hover:opacity-100"
              >
              </button>
            </div>
          {:else}
            <p class="text-[#F2F2F2] text-md md:text-lg mb-4 pl-10">
              {tempPassword}
            </p>
          {/if}
          <div class="absolute bottom-0 left-0 w-full border-t-2 border-white border-opacity-20"></div>
        </div>

        <!-- Bank Section -->
        <div class="w-full max-w-2xl text-left mb-4 relative px-4 md:px-0" in:fly={{ y: 20, duration: 400, delay: 300 }}>
          <p class="text-[#F2F2F2] text-lg md:text-xl font-normal leading-none mb-4">
            ธนาคาร
          </p>
          {#if isEditing}
            <div class="space-y-2">
              <select
                class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80 font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
                bind:value={tempBank}
              >
              <option value="" disabled selected>เลือกธนาคาร</option>
              <option value="ธนาคารกรุงเทพ">ธนาคารกรุงเทพ</option>
              <option value="ธนาคารกสิกรไทย">ธนาคารกสิกรไทย</option>
              <option value="ธนาคารไทยพาณิชย์">ธนาคารไทยพาณิชย์</option>
              <option value="ธนาคารกรุงไทย">ธนาคารกรุงไทย</option>
              <option value="ธนาคารทหารไทย">ธนาคารทหารไทย</option>
              <option value="ธนาคารยูโอบี">ธนาคารยูโอบี</option>
              </select>
              <input
                class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80 font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300"
                bind:value={tempAccount}
                placeholder="กรอกเลขบัญชี"
                type="text"
                pattern="[0-9-]*"
              />
            </div>
          {:else}
            <p class="text-[#F2F2F2] text-md md:text-lg mb-4 pl-10">
              {tempBank}
            </p>
            <p class="text-[#F2F2F2] text-md md:text-lg mb-4 pl-10">
              {tempAccount}
            </p>
          {/if}
          <div class="absolute bottom-0 left-0 w-full border-t-2 border-white border-opacity-20"></div>
        </div>

        <div class="w-full max-w-2xl text-left mb-4 relative px-4 md:px-0" in:fly={{ y: 20, duration: 400, delay: 300 }}>
          <p class="text-[#F2F2F2] text-lg md:text-xl font-normal leading-none mb-4">
            ที่อยู่
          </p>
          {#if isEditing}
            <textarea
              class="w-full py-1 px-6 rounded-xl border-2 border-[#404040] text-[#8B8B8C]
                               text-left placeholder-[#8B8B8C] placeholder-opacity-80  font-jeju text-lg md:text-xl font-normal
                               focus:outline-none focus:ring-2 focus:ring-[#546882] transition-all duration-300 min-h-[100px]"
              bind:value={tempAddress}
              placeholder="กรอกที่อยู่"
            ></textarea>
          {:else}
            <p class="text-[#F2F2F2] text-md md:text-lg mb-4 pl-10">
              {tempAddress}
            </p>
          {/if}
        </div>
  
        <!-- Logout Button -->
        <div class="flex justify-center" in:fly={{ y: 20, duration: 300, delay: 800 }}>
          <button
            class="bg-white bg-opacity-10 hover:bg-opacity-20 text-white py-2 px-8 rounded-xl
                   border border-white border-opacity-30 transition-all duration-300 hover:scale-105
                   flex items-center space-x-2"
            onclick={logout}
          >
            <span>ออกจากระบบ</span>
          </button>
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
        <div class="w-12 h-12 bg-[#404040] rounded-full flex items-center justify-center">
          <img src="/images/confirm.svg" alt="Confirm" class="w-8 h-8" />
        </div>
      </button>
    {:else}
      <button 
        onclick={toggleEdit} 
        class="fixed bottom-8 right-8 md:bottom-12 md:right-12 border-none p-0 transform transition-all duration-300 hover:scale-110"
        in:fade={{ duration: 300, delay: 100 }}
      >
        <div class="w-12 h-12 bg-[url('/images/editbg.svg')] bg-cover flex items-center justify-center">
          <img src="/images/edit.svg" alt="Edit" class="p-1 w-8 h-8" />
        </div>
      </button>
    {/if}
  </div>