<script>
  import { onMount, onDestroy } from "svelte";
  import { page } from "$app/stores";
  import { fade, fly } from "svelte/transition";
    import { goto } from "$app/navigation";

  const Api_url = "http://localhost:3000";

  let rent = 0;
  let water = 0;
  let electricity = 0;
  let lateFee = 0;
  let isOpen = true;
  let date = null;
  let roomNumber = "";

  let slip_url = "";

  const closeModal = () => {
    isOpen = false;
  };

  $: total = rent + water + electricity + lateFee;

  let room_id = "";
  let bill_id = 0;
  let payment_id = 0;
  let status = "";

  onMount(async () => {
    const unsubscribe = page.subscribe(($page) => {
      room_id = $page.url.searchParams.get("id") || "";
    });

    onDestroy(unsubscribe);

    try {
      const res = await fetch(`${Api_url}/api/get/payment?room_id=${room_id}`, {
        method: "GET",
        credentials: "include",
      });
      const data = await res.json();
      payment_id = data[0].id;
      bill_id = data[0].bill_id;
      slip_url = data[0].proof_of_payment;
      date = data[0].created_at;
      date = date.split(" ")[0];
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
    try {
      const res = await fetch(`${Api_url}/api/get/bill?bill_id=${bill_id}`, {
          method: "GET",
          credentials: "include",
        }
      );
      const data = await res.json();
      rent = data.rent_amount;
      water = data.water_fee;
      electricity = data.electricity_fee;
      lateFee = data.other_fees;
      total = data.total_amount;
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
    try {
      const res = await fetch(`${Api_url}/api/get/room?room_id=${room_id}`, {
        method: "GET",
        credentials: "include",
      });
      const data = await res.json();
      roomNumber = data.room_number;
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });

  const updateStatus = async () => {
    try {
      const res = await fetch(`${Api_url}/api/status/payment`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
        body: JSON.stringify({
          id: payment_id,
          status
        }),
      });
      const data = await res.json();
      if (data.message === "อัพเดทสถานะการชำระเงินสำเร็จ") {
        alert("อัพเดทสถานะการชำระเงินสำเร็จ");
        goto("/host/room");
      }
    } catch (error) {
      console.error("Error fetching message:", error);
    }
  };
</script>

<div class="p-4 w-full min-h-screen" in:fade={{ duration: 300 }}>
  <div class="ml-3 mb-6" in:fly={{ y: -20, duration: 400 }}>
    <p class="font-semibold text-2xl text-white mb-1">ตรวจสอบรายการชำระเงิน</p>
    <p class="font-bold text-4xl md:text-5xl text-white">{roomNumber}</p>
  </div>

  <div
    class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6"
    in:fly={{ y: 20, duration: 400, delay: 200 }}
  >
    <!-- Green card - Rent -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
        <div class="flex flex-col">
          <div class="px-3 text-lg sm:text-xl text-[#557B55]">ค่าเช่า</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
            {rent}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Blue card - Electricity -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#546882]"></div>
        <div class="flex flex-col">
          <div class="px-3 text-lg sm:text-xl text-[#546882]">ค่าไฟ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#546882]">
            {electricity}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#546882]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Orange card - Water -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#D0854C]"></div>
        <div class="flex flex-col">
          <div class="px-3 text-lg sm:text-xl text-[#D0854C]">ค่าน้ำ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D0854C]">
            {water}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#D0854C]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Red card - Late Fee -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#D04C4C]"></div>
        <div class="flex flex-col">
          <div class="px-3 text-lg sm:text-xl text-[#D04C4C]">
            ค่าชำระล่าช้า หรือ อื่นๆ
          </div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D04C4C]">
            {lateFee}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#D04C4C]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Total Amount -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 sm:col-span-1 md:col-span-2 transform transition-all duration-300 hover:scale-105"
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
        <div class="flex flex-col">
          <div class="px-3 text-lg sm:text-xl text-[#557B55]">รวมยอดชำระ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
            {total}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Payment Date -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 sm:col-span-1 md:col-span-2 transform transition-all duration-300 hover:scale-105"
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
        <div class="flex flex-col">
          <div class="px-3 text-lg sm:text-xl text-[#557B55]">วันที่ชำระ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
            {date}
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Slip Section -->
  <div
    class="flex flex-col items-center justify-center shadow rounded-xl bg-[#D9D9D9] py-6 mx-auto max-w-md md:max-w-lg lg:max-w-xl mb-6"
    in:fly={{ y: 20, duration: 400, delay: 300 }}
  >
    <img src="{slip_url}" alt="slip" class="max-h-[500px] w-auto" />
  </div>

  <div
    class="flex justify-center"
    in:fly={{ y: 20, duration: 400, delay: 400 }}
  >
    <div class="flex flex-col sm:flex-row gap-4 sm:gap-10 mt-4">
      <button
        class="rounded-2xl text-white bg-[#557B55] py-2 px-12 transform transition-all duration-300 hover:scale-105 hover:bg-[#446644] focus:outline-none focus:ring-2 focus:ring-[#557B55] focus:ring-offset-2 focus:ring-offset-[#404040]"
        on:click={() => {
          status = "approved";
          updateStatus();
        }}
      >
        ยืนยัน
      </button>
      <button
        class="rounded-2xl text-white bg-[#D04C4C] py-2 px-12 transform transition-all duration-300 hover:scale-105 hover:bg-[#b33e3e] focus:outline-none focus:ring-2 focus:ring-[#557B55] focus:ring-offset-2 focus:ring-offset-[#404040]"
        on:click={() => {
          status = "rejected";
          updateStatus();
        }}
      >
        ยกเลิก
      </button>
    </div>
  </div>
</div>
