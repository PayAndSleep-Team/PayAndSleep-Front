<script>
  import { goto } from "$app/navigation";
  import { fade, fly, scale } from "svelte/transition";

  const Api_url = "http://localhost:3000";

  let name = $state("");
  let address = $state("");
  let water_rate = $state(0);
  let electricity_rate = $state(0);
  let bank_name = $state("");
  let bank_account_number = $state("");
  let account_holder_name = $state("");
  let promptpay_number = $state("");
  let isSubmitting = $state(false);

  async function createProperty() {
    if (!validateForm()) return;

    isSubmitting = true;
    try {
      const res = await fetch(`${Api_url}/api/create/property`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          name,
          address,
          water_rate: Number(water_rate),
          electricity_rate: Number(electricity_rate),
          bank_name,
          bank_account_number,
          account_holder_name,
          promptpay_number,
        }),
        credentials: "include",
      });
      const data = await res.json();

      if (data.message === "สร้าง Property สำเร็จ") {
        showSuccessMessage();
        setTimeout(() => goto("/"), 1500);
      } else {
        alert(data.message);
      }
    } catch (error) {
      console.error("Error creating property:", error);
      alert("เกิดข้อผิดพลาดในการสร้างที่พัก กรุณาลองใหม่อีกครั้ง");
    } finally {
      isSubmitting = false;
    }
  }

  function validateForm() {
    if (!name) {
      alert("กรุณากรอกชื่อที่พัก");
      return false;
    }
    if (!address) {
      alert("กรุณากรอกที่อยู่");
      return false;
    }
    if (!water_rate) {
      alert("กรุณากรอกเรทค่าน้ำ");
      return false;
    }
    if (!electricity_rate) {
      alert("กรุณากรอกเรทค่าไฟ");
      return false;
    }
    return true;
  }

  function showSuccessMessage() {
    const successMessage = document.createElement("div");
    successMessage.className =
      "fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50";
    successMessage.innerHTML = `
            <div class="bg-white p-6 rounded-xl shadow-lg flex flex-col items-center">
                <svg class="w-16 h-16 text-green-500 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                </svg>
                <p class="text-xl font-medium">สร้างที่พักสำเร็จ</p>
                <p class="text-gray-500 mt-2">กำลังนำคุณไปยังหน้าหลัก...</p>
            </div>
        `;
    document.body.appendChild(successMessage);
    setTimeout(() => {
      document.body.removeChild(successMessage);
    }, 1500);
  }
</script>

<div
  class="w-full min-h-screen flex flex-col items-center justify-center p-4"
  in:fade={{ duration: 300 }}
>
  <div
    class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-full max-w-2xl shadow-lg flex flex-col items-center py-8 px-6"
    in:scale={{ duration: 400, delay: 100 }}
  >
    <h1
      class="text-[#404040] text-center font-jeju text-3xl md:text-4xl font-normal mb-6"
      in:fly={{ y: -20, duration: 400, delay: 200 }}
    >
      สร้างที่พัก
    </h1>

    <form
      class="w-full max-w-md space-y-4"
      in:fly={{ y: 20, duration: 400, delay: 300 }}
    >
      <!-- Name -->
      <div class="transition-all duration-300 hover:scale-[1.01]">
        <input
          type="text"
          id="name"
          name="name"
          placeholder="ชื่อที่พัก"
          bind:value={name}
          class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
        />
      </div>

      <!-- Address -->
      <div class="transition-all duration-300 hover:scale-[1.01]">
        <textarea
          id="address"
          name="address"
          placeholder="ที่อยู่"
          rows="3"
          bind:value={address}
          class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
        ></textarea>
      </div>

      <!-- Rates Section -->
      <!-- Water Rate -->
      <div class="transition-all duration-300 hover:scale-[1.01]">
        <input
          type="number"
          id="water_rate"
          name="water_rate"
          placeholder="เรทค่าน้ำ (บาท/หน่วย)"
          value={water_rate || ""}
          oninput={(e) => (water_rate = e.target.value)}
          class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
        />
      </div>

      <!-- Electricity Rate -->
      <div class="transition-all duration-300 hover:scale-[1.01]">
        <input
          type="number"
          id="electricity_rate"
          name="electricity_rate"
          placeholder="เรทค่าไฟ (บาท/หน่วย)"
          value={electricity_rate || ""}
          oninput={(e) => (electricity_rate = e.target.value)}
          class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
        />
      </div>

      <!-- Bank Section -->
      <div class="pt-2">
        <h2 class="text-[#404040] font-medium text-lg mb-3">
          ข้อมูลการชำระเงิน
        </h2>

        <!-- Bank Name -->
        <div
          class="relative mb-4 transition-all duration-300 hover:scale-[1.01]"
        >
          <select
            id="bank_name"
            name="bank_name"
            bind:value={bank_name}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal {bank_name
              ? 'text-[#404040]'
              : 'text-[#8B8B8C]'} appearance-none focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          >
            <option value="" disabled selected>ธนาคาร</option>
            <option value="ธนาคารกรุงเทพ">ธนาคารกรุงเทพ</option>
            <option value="ธนาคารกสิกรไทย">ธนาคารกสิกรไทย</option>
            <option value="ธนาคารไทยพาณิชย์">ธนาคารไทยพาณิชย์</option>
            <option value="ธนาคารกรุงไทย">ธนาคารกรุงไทย</option>
            <option value="ธนาคารทหารไทย">ธนาคารทหารไทย</option>
            <option value="ธนาคารยูโอบี">ธนาคารยูโอบี</option>
          </select>
          <div
            class="absolute inset-y-0 right-5 flex items-center pointer-events-none text-[#404040]"
          >
            ▼
          </div>
        </div>

        <!-- Bank Account Number -->
        <div class="mb-4 transition-all duration-300 hover:scale-[1.01]">
          <input
            type="text"
            id="bank_account_number"
            name="bank_account_number"
            placeholder="เลขบัญชีธนาคาร"
            bind:value={bank_account_number}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>

        <!-- Account Holder Name -->
        <div class="mb-4 transition-all duration-300 hover:scale-[1.01]">
          <input
            type="text"
            id="account_holder_name"
            name="account_holder_name"
            placeholder="ชื่อบัญชีธนาคาร"
            bind:value={account_holder_name}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>

        <!-- PromptPay Number -->
        <div class="transition-all duration-300 hover:scale-[1.01]">
          <input
            type="text"
            id="promptpay_number"
            name="promptpay_number"
            placeholder="หมายเลขพร้อมเพย์"
            bind:value={promptpay_number}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
      </div>

      <!-- Create Property Button -->
      <button
        type="button"
        onclick={createProperty}
        disabled={isSubmitting}
        class="w-full py-3 mt-6 rounded-xl text-xl cursor-pointer bg-[#404040] border-none text-[#F2F2F2] hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2 disabled:opacity-70 disabled:cursor-not-allowed"
      >
        {#if isSubmitting}
          <span class="flex items-center justify-center">
            <svg
              class="animate-spin -ml-1 mr-3 h-5 w-5 text-white"
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
            กำลังสร้างที่พัก...
          </span>
        {:else}
          สร้างที่พัก
        {/if}
      </button>
    </form>
  </div>
</div>
