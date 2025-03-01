<script>
    import { goto } from "$app/navigation";

    const Api_url = "http://localhost:3000";

    let name = $state("");
    let address = $state("");
    let water_rate = $state(0);
    let electricity_rate = $state(0);
    let bank_name = $state("");
    let bank_account_number = $state("");
    let account_holder_name = $state("");
    let promptpay_number = $state("");

    async function createProperty() {
        try {
            const res = await fetch(`${Api_url}/api/create/property`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    name: name,
                    address: address,
                    water_rate: water_rate,
                    electricity_rate: electricity_rate,
                    bank_name: bank_name,
                    bank_account_number: bank_account_number,
                    account_holder_name: account_holder_name,
                    promptpay_number: promptpay_number
                }),
                credentials: "include",
            });
            const data = await res.json();
            alert(data.message);

            if (data.message === "สร้าง Property สำเร็จ") {
                goto("/");
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    }
</script>

<div class="w-full h-screen flex flex-col items-center justify-center">
    <!-- create property -->
    <div class="bg-[rgba(242,242,242,0.8)] rounded-2xl w-[45%] h-[95%] shadow-lg flex flex-col items-center justify-center">
      <h1 class="text-[#404040] text-center font-jeju text-4xl font-normal mb-9">สร้างที่พัก</h1>
      <form class="w-[90%] max-w-[550px] min-w-[400px] mt-4">
        <!-- Name -->
        <div class="mb-4">
          <input
            type="text"
            id="name"
            name="name"
            placeholder="ชื่อ"
            bind:value={name}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <!-- Address -->
        <div class="mb-4">
          <textarea
            id="address"
            name="address"
            placeholder="ที่อยู่"
            rows="4"
            bind:value={address}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          ></textarea>
        </div>
        <!-- Water Rate -->
        <div class="mb-4">
          <input
            type="number"
            id="water_rate"
            name="water_rate"
            placeholder="เรทค่าน้ำ"
            bind:value={water_rate}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <!-- Electricity Rate -->
        <div class="mb-4">
          <input
            type="number"
            id="electricity_rate"
            name="electricity_rate"
            placeholder="เรทค่าไฟ"
            bind:value={electricity_rate}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <!-- Bank Name -->
        <div class="mb-4">
          <select
            id="bank_name"
            name="bank_name"
            bind:value={bank_name}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          >
            <option value="" disabled selected>ธนาคาร</option>
            <option value="ธนาคารกรุงเทพ">ธนาคารกรุงเทพ</option>
            <option value="ธนาคารกสิกรไทย">ธนาคารกสิกรไทย</option>
            <option value="ธนาคารไทยพาณิชย์">ธนาคารไทยพาณิชย์</option>
            <option value="ธนาคารกรุงไทย">ธนาคารกรุงไทย</option>
            <option value="ธนาคารทหารไทย">ธนาคารทหารไทย</option>
            <option value="ธนาคารยูโอบี">ธนาคารยูโอบี</option>
          </select>
        </div>
        <!-- Bank Account Number -->
        <div class="mb-4">
          <input
            type="text"
            id="bank_account_number"
            name="bank_account_number"
            placeholder="เลขบัญชีธนาคาร"
            bind:value={bank_account_number}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <!-- Account Holder Name -->
        <div class="mb-4">
          <input
            type="text"
            id="account_holder_name"
            name="account_holder_name"
            placeholder="ชื่อบัญชีธนาคาร"
            bind:value={account_holder_name}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <!-- PromptPay Number -->
        <div class="mb-4">
          <input
            type="text"
            id="promptpay_number"
            name="promptpay_number"
            placeholder="หมายเลขพร้อมเพย์"
            bind:value={promptpay_number}
            class="w-full py-2.5 px-6 rounded-[25px] border-2 border-[#404040] bg-transparent text-left placeholder-[#8B8B8C] font-jeju text-xl font-normal"
          />
        </div>
        <!-- Create Property Button -->
        <button
          type="button"
          on:click={createProperty}
          class="w-full py-3 mt-9 rounded-[25px] text-xl cursor-pointer bg-[#404040] border-none text-[#D9D9D9]"
        >
          สร้างที่พัก
        </button>
      </form>
    </div>
  </div>