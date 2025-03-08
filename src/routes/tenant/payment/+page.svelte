<script>
  import { page } from "$app/stores";
  import { onDestroy, onMount } from "svelte";
  import DragAndDrop from "$lib/components/DragAndDrop.svelte";
  import Dropdown from "$lib/components/Dropdown.svelte";
  import { fade, fly, scale } from "svelte/transition";
  import { goto } from "$app/navigation";
    import { base } from "$app/paths";

  const Api_url = "http://localhost:3000";

  let rent = 4500;
  let water = 1389;
  let electricity = 95;
  let lateFee = 20;
  let isOpen = true;
  let selectedFile = null;
  let paymentMethod = "เลือกวิธีชำระเงิน";
  let total = 0;

  let bill_id = "";
  let property = {};

  onMount(async () => {
    const unsubscribe = page.subscribe(($page) => {
      bill_id = $page.url.searchParams.get("bill_id") || "";
    });

    onDestroy(unsubscribe);

    try {
      const res = await fetch(
        `${Api_url}/api/get/bill-paymentmethod?bill_id=${bill_id}`,
        {
          method: "GET",
          credentials: "include",
        },
      );
      const data = await res.json();
      const bill = data.bill;
      property = data.property;
      rent = bill.rent_amount;
      water = bill.water_fee;
      electricity = bill.electricity_fee;
      lateFee = bill.other_fees;
      total = bill.total_amount;
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });

  let method = "";

  const submitPayment = async () => {
    try {
      if (paymentMethod === "เลือกวิธีชำระเงิน") {
        alert("กรุณาเลือกวิธีชำระเงิน");
        return;
      } else if (!selectedFile) {
        alert("กรุณาอัพโหลดสลิปการโอนเงิน");
        return;
      }
      if (paymentMethod === "พร้อมเพย์") {
        method = "QR PromptPay";
      } else if (paymentMethod === "บัญชีธนาคาร") {
        method = "Bank Transfer";
      }
      const res = await fetch(`${Api_url}/api/create/payment`, {
        method: "POST",
        credentials: "include",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          bill_id,
          amount: total,
          payment_method: method,
          proof_of_payment: selectedFile,
        }),
      });
      const data = await res.json();
      if (data.message === "สร้างการชำระเงินสำเร็จ") {
        try {
          const res = await fetch(`${Api_url}/api/pay/bill`, {
            method: "PUT",
            credentials: "include",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              bill_id,
            }),
          });
          const data2 = await res.json();
          if (data2.message === "ชำระเงินสำเร็จ") {
            alert("ชำระเงินสำเร็จ");
            goto("/tenant/paymentstatus");
          }
        } catch (error) {
          console.error("Error fetching message:", error);
        }
      }
    } catch (error) {
      console.error("Error fetching message:", error);
    }
  };

  const closeModal = () => {
    isOpen = false;
  };

  $: if (paymentMethod === "พร้อมเพย์") {
    isOpen = true;
  }
</script>

<div class="p-4 w-full" in:fade={{ duration: 300 }}>
  <p
    class="font-bold text-xl md:text-2xl text-white mb-2"
    in:fly={{ y: -20, duration: 600 }}
  >
    ค่าบริการรายเดือน
  </p>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-4">
    <!-- Green card -->
    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
      in:fly={{ x: -20, duration: 600, delay: 100 }}
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
        <div>
          <div class="px-3 text-lg sm:text-xl text-[#557B55]">ค่าเช่า</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
            {rent}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Blue card -->
    <div
      class="bg-white rounded-xl min-h-28 max-h-28 shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
      in:fly={{ x: -20, duration: 600, delay: 100 }}
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#546882]"></div>
        <div>
          <div class="px-3 text-lg sm:text-xl text-[#546882]">ค่าไฟ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#546882]">
            {electricity}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#546882]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Orange card -->
    <div
      class="bg-white rounded-xl max-h-28 shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
      in:fly={{ x: -20, duration: 600, delay: 100 }}
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#D0854C]"></div>
        <div>
          <div class="px-3 text-lg sm:text-xl text-[#D0854C]">ค่าน้ำ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D0854C]">
            {water}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#D0854C]">บาท</div>
        </div>
      </div>
    </div>

    <!-- Red card -->
    <div
      class="bg-white rounded-xl min-h-28 max-h-28 shadow overflow-hidden p-2 transform transition-all duration-300 hover:scale-105"
      in:fly={{ x: -20, duration: 600, delay: 100 }}
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#D04C4C]"></div>
        <div>
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

    <div
      class="bg-white rounded-xl shadow overflow-hidden p-2 col-span-1 sm:col-span-2 transform transition-all duration-300 hover:scale-105"
      in:fly={{ x: 20, duration: 600, delay: 500 }}
    >
      <div class="flex items-center h-full">
        <div class="h-full w-[11px] ml-1 my-2 bg-[#557B55]"></div>
        <div>
          <div class="px-3 text-lg sm:text-xl text-[#557B55]">รวมยอดชำระ</div>
          <div class="px-3 text-2xl sm:text-4xl font-bold text-[#557B55]">
            {total}
          </div>
          <div class="px-3 text-base sm:text-lg text-[#557B55]">บาท</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Payment section -->
  <div
    class="flex flex-col items-center justify-center rounded-xl text-white relative p-4 md:p-6"
    in:fly={{ y: 20, duration: 600, delay: 600 }}
  >
    <p class="text-xl md:text-2xl mt-4 font-semibold text=[#404040] mb-4">
      ชำระเงิน
    </p>
    <div class="mb-4 text-lg md:text-xl w-full max-w-md">
      <Dropdown bind:value={paymentMethod} />
    </div>
    {#if paymentMethod === "พร้อมเพย์"}
      {#if isOpen}
        <div
          class="p-4 md:p-5 fixed inset-0 flex items-center justify-center z-50 bg-black bg-opacity-50"
          transition:fade={{ duration: 200 }}
        >
          <div
            class="bg-[#404040] rounded-xl p-4 md:p-6 relative max-w-lg w-full mx-4"
            transition:scale={{ duration: 300, start: 0.95 }}
          >
            <button
              onclick={closeModal}
              class="absolute right-2 top-2 transition-transform hover:scale-110"
            >
              <img src="/images/cross.svg" alt="cross" class="w-6 h-6" />
            </button>

            <div class="text-center">
              <p class="text-lg md:text-xl">พร้อมเพย์</p>
              <p class="text-lg md:text-xl mb-2">{property.promptpay_number}</p>
            </div>

            <img
              src="https://promptpay.io/{property.promptpay_number}"
              alt="qrCode"
              class="w-full max-w-sm mx-auto"
            />
          </div>
        </div>
      {/if}
    {/if}

    {#if paymentMethod === "บัญชีธนาคาร"}
      <div
        class="flex items-center justify-center flex-col text-white text-lg md:text-xl mb-4"
        in:fade={{ duration: 300 }}
      >
        <p>{property.bank_name}</p>
        <p>{property.bank_account_number} {property.account_holder_name}</p>
      </div>
    {/if}

    <div
      class="p-4 md:p-8 rounded-xl bg-[#D9D9D9] bg-opacity-80 flex flex-col justify-center items-center w-full max-w-2xl"
    >
      <DragAndDrop bind:file={selectedFile} />
    </div>

    <div class="flex gap-4 mt-4">
      <button
        class="rounded-xl text-[#F2F2F2] bg-[#404040] py-2 px-6 md:px-8 transition-all duration-300 hover:scale-105"
        onclick={submitPayment}
      >
        ยืนยัน
      </button>
      <button
        class="rounded-xl text-[#404040] bg-[#F2F2F2] py-2 px-6 md:px-8 transition-all duration-300 hover:scale-105"
      >
        ยกเลิก
      </button>
    </div>
  </div>
</div>
