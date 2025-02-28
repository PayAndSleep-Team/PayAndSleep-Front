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
          body: JSON.stringify({
            email: email,
            password: password,
          }),
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
  
<style>
    @font-face {
      font-family: 'JejuGothic';
      src: url('/fonts/JejuGothic-Regular.ttf') format('truetype');
    }
  
    .bg-image {
      background-image: url('/images/bg.png');
      background-size: cover;
      background-position: center;
      width: 100vw;
      height: 100vh;
      color: white;
      display: flex;
      justify-content: flex-end;
      align-items: center;
    }
  
    .login-box {
      background-color: #F2F2F2CC;
      border-radius: 20px;
      width: 48.5%;
      height: 93vh;
      flex-shrink: 0;
      margin-right: 35px;
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
      padding: 30px;
      display: flex;
      flex-direction: column;
    align-items: center;
    justify-content: center;
    }
  
    .login-box h1 {
      color: #404040;
      text-align: center;
      font-family: 'JejuGothic', sans-serif;
      font-size: 40px;
      font-style: normal;
      font-weight: 400;
      line-height: normal;
      margin-bottom: 30px;
    }
  
    .input-container {
      position: relative;
      margin-bottom: 25px;
      width: 100%;
    }
  
    .input-container input {
      width: 100%;
      padding: 10px 20px;
      border-radius: 25px;
      border: 2px solid #404040;
      box-sizing: border-box;
      background-color: transparent;
      font-family: 'JejuGothic';
      font-size: 20px;
      color: #404040;
    }

    input::placeholder {
    color: #8B8B8C;
    font-family: 'JejuGothic';
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    }

  
    .login-button {
      background-color: #404040;
      color: white;
      padding: 15px 35px;
      border: none;
      border-radius: 25px;
      cursor: pointer;
      font-family: 'JejuGothic';
      font-size: 22px;
      font-weight: 500;
      margin-top: 20px;
    }
  
    .register-button {
      background-color: transparent;
      color: #404040;
      border: none;
      cursor: pointer;
      font-family: 'JejuGothic';
      font-size: 18px;
      margin-top: 10px;
      display: block;
      width: fit-content;
      margin: 10px auto;
    }
</style>
  
<div class="bg-image">
    {#if !check}
      <div class="login-box">
        <h1 class="text-2xl font-bold text-center">เข้าสู่ระบบ</h1>
        <form class="mt-4 w-100" on:submit={login}>
          <div class="input-container">
            <input type="email" id="email" name="email" placeholder="อีเมล" bind:value={email} required />
          </div>
          <div class="input-container">
            <input type="password" id="password" name="password" placeholder="รหัสผ่าน" bind:value={password} required />
          </div>
          <button type="submit" class="login-button">
            เข้าสู่ระบบ
          </button>
          <button type="button" class="register-button" on:click={showPopup}>
            ลงทะเบียน
          </button>
        </form>
      </div>
    {:else}
      <div class="w-96 bg-white p-8 rounded-lg shadow-lg">
        <h1 class="text-2xl font-bold text-center">คุณต้องการลงทะเบียนในสถานะใด</h1>
        <div class="flex justify-between items-center mt-4">
          <button type="button" class="bg-indigo-500 text-white px-4 py-2 rounded-md" on:click={() => goto("/register?role=host")}>
            Host
          </button>
          <button type="button" class="bg-indigo-500 text-white px-4 py-2 rounded-md" on:click={() => goto("/register?role=tenant")}>
            Tenant
          </button>
        </div>
      </div>
    {/if}
</div>