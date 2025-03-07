<script>
    import { goto } from "$app/navigation";
    import { page } from '$app/stores';
    import { fade, fly, scale } from 'svelte/transition';
  
    let role = $page.url.searchParams.get('role');
  
    const Api_url = "http://localhost:3000";
    let lst = [];
    let message = $state("loading...");
    let email = $state("");
    let name = $state("");
    let phone = $state("");
    let password = $state("");
    let isLoading = $state(false);
    let errorMessage = $state("");
  
    async function register() {
      errorMessage = "";
      
      if (email === "" || password === "" || role === "" || name === "" || phone === "") {
        errorMessage = "กรุณากรอกข้อมูลให้ครบ";
        return;
      }
      
      isLoading = true;
      try {
        const res = await fetch(`${Api_url}/api/register`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            email: email,
            password: password,
            role: role,
            name: name,
            phone: phone,
          }),
        });
        const data = await res.json();
        
        if (data.message === "ลงทะเบียนสำเร็จ") {
          showSuccessMessage();
          setTimeout(() => goto("/landing/login"), 1500);
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
    
    function showSuccessMessage() {
      const successMessage = document.createElement('div');
      successMessage.className = 'fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50';
      successMessage.innerHTML = `
        <div class="bg-white p-6 rounded-xl shadow-lg flex flex-col items-center">
          <svg class="w-16 h-16 text-green-500 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
          </svg>
          <p class="text-xl font-medium">ลงทะเบียนสำเร็จ</p>
          <p class="text-gray-500 mt-2">กำลังนำคุณไปยังหน้าเข้าสู่ระบบ...</p>
        </div>
      `;
      document.body.appendChild(successMessage);
      setTimeout(() => {
        document.body.removeChild(successMessage);
      }, 1500);
    }
  </script>
  
<div class="w-full h-screen flex flex-col items-center justify-center" in:fade={{ duration: 300 }}>
  <div 
    class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[90%] md:w-[70%] lg:w-[45%] h-auto py-10 shadow-lg flex flex-col items-center justify-center"
    in:scale={{ duration: 400, delay: 100 }}
  >
    <h1 
      class="text-[#404040] text-center font-jeju text-3xl md:text-4xl font-normal mb-6"
      in:fly={{ y: -20, duration: 400, delay: 200 }}
    >
      ลงทะเบียน {role === 'host' ? 'เจ้าของที่พัก' : 'ผู้เช่า'}
    </h1>
    
    {#if errorMessage}
      <div 
        class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-xl mb-4 w-[90%] max-w-[550px]"
        in:fly={{ y: 20, duration: 300 }}
      >
        <p>{errorMessage}</p>
      </div>
    {/if}
    
    <form class="w-[90%] max-w-[550px]" in:fly={{ y: 20, duration: 400, delay: 300 }}>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
        <div class="transition-all duration-300 hover:scale-[1.01]">
          <input
            type="text"
            id="name"
            name="name"
            placeholder="ชื่อ - นามสกุล"
            bind:value={name}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
        <div class="transition-all duration-300 hover:scale-[1.01]">
          <input
            type="text"
            id="phone"
            name="phone"
            placeholder="เบอร์โทรศัพท์"
            bind:value={phone}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
        <div class="transition-all duration-300 hover:scale-[1.01]">
          <input
            type="email"
            id="email"
            name="email"
            placeholder="อีเมล"
            bind:value={email}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
        <div class="transition-all duration-300 hover:scale-[1.01]">
          <input
            type="password"
            id="password"
            name="password"
            placeholder="รหัสผ่าน"
            bind:value={password}
            class="w-full py-3 px-6 rounded-xl border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-lg md:text-xl font-normal focus:outline-none focus:ring-2 focus:ring-[#404040] transition-all"
          />
        </div>
      </div>
      <div class="flex flex-col md:flex-row gap-4">
        <button
          type="button"
          onclick={register}
          disabled={isLoading}
          class="w-full py-3 rounded-xl text-xl cursor-pointer bg-[#404040] border-none text-white hover:bg-[#333333] transition-all duration-300 transform hover:scale-[1.02] focus:outline-none focus:ring-2 focus:ring-[#404040] focus:ring-offset-2 disabled:opacity-70 disabled:cursor-not-allowed"
        >
          {#if isLoading}
            <span class="flex items-center justify-center">
              <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              กำลังลงทะเบียน...
            </span>
          {:else}
            ลงทะเบียน
          {/if}
        </button>
      </div>
    </form>
  </div>
</div>

<style>
  @font-face {
    font-family: "JejuGothic";
    src: url("/fonts/JejuGothic-Regular.ttf") format("truetype");
  }
</style>