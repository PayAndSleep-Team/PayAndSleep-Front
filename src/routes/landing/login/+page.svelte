<script>
  import { goto } from "$app/navigation";
  import { fade, fly, scale } from 'svelte/transition';

  const Api_url = "http://localhost:3000";
  let email = "";
  let password = "";
  let check = false;
  let isLoading = false;
  let errorMessage = "";

  async function login(event) {
    event.preventDefault();
    errorMessage = "";
    isLoading = true;
    
    try {
      const res = await fetch(`${Api_url}/api/login`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ email, password }),
        credentials: "include",
      });
      const data = await res.json();
      
      if (data.message === "เข้าสู่ระบบสำเร็จ") {
        showSuccessMessage();
        setTimeout(() => goto("/"), 1500);
      } else {
        errorMessage = data.message;
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      errorMessage = "เกิดข้อผิดพลาดในการเชื่อมต่อ กรุณาลองใหม่อีกครั้ง";
    } finally {
      isLoading = false;
    }
  }

  const showPopup = () => {
    check = true;
  };
  
  const goBackToLogin = () => {
    check = false;
  };
  
  function showSuccessMessage() {
    const successMessage = document.createElement('div');
    successMessage.className = 'fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50';
    successMessage.innerHTML = `
      <div class="bg-white p-6 rounded-xl shadow-lg flex flex-col items-center">
        <svg class="w-16 h-16 text-green-500 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
        </svg>
        <p class="text-xl font-medium">เข้าสู่ระบบสำเร็จ</p>
        <p class="text-gray-500 mt-2">กำลังนำคุณไปยังหน้าหลัก...</p>
      </div>
    `;
    document.body.appendChild(successMessage);
    setTimeout(() => {
      document.body.removeChild(successMessage);
    }, 1500);
  }
</script>

<div class="w-full h-screen flex justify-center md:justify-end items-center px-4 md:pr-[5%]">
  {#if !check}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-full sm:w-[80%] md:w-[60%] lg:w-[45%] h-auto py-10 md:h-[95%] shadow-lg flex flex-col items-center justify-center"
      in:scale={{ duration: 400, delay: 100 }}
    >
      <h1
        class="text-[#404040] text-center font-jeju text-3xl md:text-4xl font-normal mb-6 md:mb-9"
        in:fly={{ y: -20, duration: 400, delay: 200 }}
      >
        เข้าสู่ระบบ
      </h1>
      
      {#if errorMessage}
        <div 
          class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-xl mb-4 w-[90%] max-w-[550px]"
          in:fly={{ y: 20, duration: 300 }}
        >
          <p>{errorMessage}</p>
        </div>
      {/if}
      
      <form on:submit={login} class="w-[90%] max-w-[550px]" in:fly={{ y: 20, duration: 400, delay: 300 }}>
        <div class="mb-4 transition-all duration-300 hover:scale-[1.01]">
          <input
            type="email"
            id="email"
            placeholder="อีเมล"
            bind:value={email}
            required
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
        <div class="mb-6 transition-all duration-300 hover:scale-[1.01]">
          <input
            type="password"
            id="password"
            placeholder="รหัสผ่าน"
            bind:value={password}
            required
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
        <button
          type="submit"
          disabled={isLoading}
          class="w-full py-3 rounded-xl text-xl cursor-pointer mt-2.5 bg-[#404040] text-white border-none hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2 disabled:opacity-70 disabled:cursor-not-allowed"
        >
          {#if isLoading}
            <span class="flex items-center justify-center">
              <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              กำลังเข้าสู่ระบบ...
            </span>
          {:else}
            เข้าสู่ระบบ
          {/if}
        </button>
        <button
          type="button"
          on:click={showPopup}
          class="w-full py-3 rounded-xl text-xl cursor-pointer mt-4 bg-transparent text-[#404040] border-2 border-[#404040] hover:bg-[#40404010] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040]"
        >
          ลงทะเบียน
        </button>
      </form>
    </div>
  {:else}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-full sm:w-[80%] md:w-[60%] lg:w-[45%] h-auto py-10 md:h-[95%] shadow-lg flex flex-col items-center justify-center"
      in:scale={{ duration: 400 }}
    >
      <h1
        class="text-[#404040] text-center font-jeju text-3xl md:text-4xl font-normal mb-8 md:mb-16 px-4"
        in:fly={{ y: -20, duration: 400, delay: 100 }}
      >
        ต้องการลงทะเบียนในสถานะใด
      </h1>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 sm:gap-8 w-[90%] max-w-[550px]" in:fly={{ y: 20, duration: 400, delay: 200 }}>
        <button
          on:click={() => goto("/landing/register?role=host")}
          class="py-4 px-8 rounded-xl text-xl cursor-pointer bg-[#404040] border-none text-white hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.05] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2"
        >
          Host
        </button>
        <button
          on:click={() => goto("/landing/register?role=tenant")}
          class="py-4 px-8 rounded-xl text-xl cursor-pointer bg-[#404040] border-none text-white hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.05] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2"
        >
          Tenant
        </button>
      </div>
      <button
        on:click={goBackToLogin}
        class="mt-8 py-2 px-6 rounded-xl text-base cursor-pointer bg-transparent text-[#404040] border border-[#404040] hover:bg-[#40404010] transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-[#404040]"
        in:fade={{ duration: 300, delay: 300 }}
      >
        กลับไปหน้าเข้าสู่ระบบ
      </button>
    </div>
  {/if}
</div>

<style>
  @font-face {
    font-family: "JejuGothic";
    src: url("/fonts/JejuGothic-Regular.ttf") format("truetype");
  }
</style>
