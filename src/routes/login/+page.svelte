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

<style>
  @font-face {
      font-family: 'JejuGothic';
      src: url('/fonts/JejuGothic-Regular.ttf') format('truetype');
  }

  * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
  }

    .bg-image {
    background-image: url('/images/bg.png');
    background-size: cover;
    background-position: center;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    padding-right: 5%;
  }


    .login-box {
    background-color: rgba(242, 242, 242, 0.8);
    border-radius: 20px;
    width: 45%;
    height: 95%;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }


  .login-box h1 {
    color: #404040;
    text-align: center;
    font-family: 'JejuGothic';
    font-size: 40px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    margin-bottom: 36px;
  }

  .input-container {
      margin-bottom: 15px;
    width: 90%; /* Adjust this as needed */
    max-width: 550px; /* Ensures a reasonable max size */
    min-width: 400px; /* Prevents it from being too small */
  }

  .input-container input {
      width: 100%;
      padding: 10px;
      border-radius: 25px;
      border: 2px solid #404040;
      background-color: transparent;
      font-size: 18px;
      text-align: left;
      padding-left: 25px;
  }

  input::placeholder {
    text-align: left;
    padding-left: 10px;
    color: #8B8B8C;
    font-family: 'JejuGothic';
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
  }

  .login-button, .register-button {
      width: 100%;
      padding: 12px;
      border-radius: 25px;
      font-size: 18px;
      cursor: pointer;
      margin-top: 10px;
  }

  .login-button {
      background-color: #404040;
      color: white;
      border: none;
  }

  .register-button {
      background: none;
      color: #404040;
      border: none;
  }

  .popup {
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
      text-align: center;
  }
</style>

<div class="bg-image">
  {#if !check}
  <div class="login-box">
      <h1>เข้าสู่ระบบ</h1>
      <form on:submit={login}>
          <div class="input-container">
              <input type="email" id="email" placeholder="อีเมล" bind:value={email} required />
          </div>
          <div class="input-container">
              <input type="password" id="password" placeholder="รหัสผ่าน" bind:value={password} required />
          </div>
          <button type="submit" class="login-button">เข้าสู่ระบบ</button>
          <button type="button" class="register-button" on:click={showPopup}>ลงทะเบียน</button>
      </form>
  </div>
  {:else}
  <div class="popup">
      <h1>คุณต้องการลงทะเบียนในสถานะใด</h1>
      <div>
          <button class="login-button" on:click={() => goto("/register?role=host")}>Host</button>
          <button class="login-button" on:click={() => goto("/register?role=tenant")}>Tenant</button>
      </div>
  </div>
  {/if}
</div>
