<script>
  import { goto } from "$app/navigation";

  const Api_url = "http://localhost:3000";
  let email = "";
  let password = "";
  let check = false;

  async function login(event) {
    event.preventDefault();
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
      alert(data.message);

      if (data.message === "เข้าสู่ระบบสำเร็จ") {
        goto("/");
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      alert("Failed to load message");
    }
  }

  const showPopup = () => {
    check = true;
  };
</script>

<div class=" w-full h-screen flex justify-end items-center pr-[5%]">
  {#if !check}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center"
    >
      <h1
        class="text-[#404040] text-center font-jeju text-4xl font-normal mb-9"
      >
        เข้าสู่ระบบ
      </h1>
      <form on:submit={login} class="w-[90%] max-w-[550px] min-w-[400px]">
        <div class="mb-4">
          <input
            type="email"
            id="email"
            placeholder="อีเมล"
            bind:value={email}
            required
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <div class="mb-4">
          <input
            type="password"
            id="password"
            placeholder="รหัสผ่าน"
            bind:value={password}
            required
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <button
          type="submit"
          class="w-full py-3 rounded-[25px] text-xl cursor-pointer mt-2.5 bg-[#404040] text-white border-none text-[#D9D9D9"
        >
          เข้าสู่ระบบ
        </button>
        <button
          type="button"
          on:click={showPopup}
          class="w-full py-3 rounded-[25px] text-xl cursor-pointer mt-2.5 bg-transparent text-[#404040] border-none"
        >
          ลงทะเบียน
        </button>
      </form>
    </div>
  {:else}
    <div
      class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center"
    >
      <h1
        class="text-[#404040] text-center font-jeju text-4xl font-normal mb-20"
      >
        ต้องการลงทะเบียนในสถานะใด
      </h1>
      <div class="grid grid-cols-2 gap-12">
        <button
          on:click={() => goto("/landing/register?role=host")}
          class="w-full py-3 px-20 rounded-[25px] text-xl cursor-pointer bg-[#404040] border-none text-[#D9D9D9]"
        >
          Host
        </button>
        <button
          on:click={() => goto("/landing/register?role=tenant")}
          class="w-full py-3 rounded-[25px] text-xl cursor-pointer bg-[#404040] border-none text-[#D9D9D9]"
        >
          Tenant
        </button>
      </div>
    </div>
  {/if}
</div>

<style>
  @font-face {
    font-family: "JejuGothic";
    src: url("/fonts/JejuGothic-Regular.ttf") format("truetype");
  }
</style>
