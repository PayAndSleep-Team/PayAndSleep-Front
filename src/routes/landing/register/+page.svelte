<script>
    import { goto } from "$app/navigation";
    import { page } from '$app/stores';
  
    let role = $page.url.searchParams.get('role');
    console.log(role);
  
    const Api_url = "http://localhost:3000";
    let lst = [];
    let message = $state("loading...");
    let email = $state("");
    let name = $state("");
    let phone = $state("");
    let password = $state("");
    let confirm_password = $state("");
  
    async function register() {
      if (password !== confirm_password) {
        alert("Password and Confirm Password do not match");
        return;
      }
      if (email === "" || password === "" || confirm_password === "" || role === "" || name === "" || phone === "") {
        alert("Please fill all fields");
        return;
      }
      console.log(email, password, role, name, phone);
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
        alert(data.message);
  
        if (data.message === "ลงทะเบียนสำเร็จ") {
          goto("/login");
        }
      } catch (error) {
        console.error("Error fetching message:", error);
        message = "Failed to load message";
      }
    }
  </script>
  
<div class="w-full h-screen flex flex-col items-center justify-center">
  <div class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center">
    <h1 class="text-[#404040] text-center font-jeju text-4xl font-normal mb-9">ลงทะเบียน</h1>
    <form class="w-[90%] max-w-[550px] min-w-[400px]">
      <div class="grid grid-cols-2 gap-4 mb-4">
        <div>
          <input
            type="text"
            id="name"
            name="name"
            placeholder="ชื่อ - นามสกุล"
            bind:value={name}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <div>
          <input
            type="text"
            id="phone"
            name="phone"
            placeholder="เบอร์โทรศัพท์"
            bind:value={phone}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <div>
          <input
            type="email"
            id="email"
            name="email"
            placeholder="อีเมล"
            bind:value={email}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <div>
          <input
            type="password"
            id="password"
            name="password"
            placeholder="รหัสผ่าน"
            bind:value={password}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
      </div>
      <div class="mb-4">
        <input
          type="password"
          id="confirm_password"
          name="confirm_password"
          placeholder="ยืนยันรหัสผ่าน"
          bind:value={confirm_password}
          class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
        />
      </div>
      <button
        type="button"
        on:click={register}
        class="w-full py-3 mt-9 rounded-[25px] text-xl cursor-pointer bg-[#404040] border-none text-[#D9D9D9]"
      >
        ลงทะเบียน
      </button>
    </form>
  </div>
</div>