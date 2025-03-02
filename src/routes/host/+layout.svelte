<script>
  /** @type {{ data: import('./$types').LayoutData, children: import('svelte').Snippet }} */
  let { data, children } = $props();

  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  const Api_url = "http://localhost:3000";
  let user = $state({});

  onMount(async () => {
    try {
      const res = await fetch(`${Api_url}/api/session`, {
        method: "GET",
        credentials: "include",
      });
      user = await res.json();
      if (user.role !== "host") {
        goto("/login");
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });
</script>

<div class="flex min-h-screen">
  <div
    class="sidebar bg-[#404040] text-white py-8 my-5 mx-4 flex flex-col rounded-2xl items-center w-[12%]"
  >
    <div>
      <div class="flex flex-col items-center mb-4">
        <a href="/" class="block py-4 mb-3 px-4 text-xl">หน้าหลัก</a>
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/profile" class="block py-4 mb-3 px-4 text-xl">ข้อมูลส่วนตัว</a
        >
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/notifications" class="block py-4 mb-3 px-4 text-xl"
          >การแจ้งเตือน</a
        >
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/requests" class="block py-4 mb-3 px-4 text-xl"
          >ส่งคำร้อง<br />ช่องบำรุง</a
        >
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/payment" class="block py-4 mb-3 px-4 text-xl">ชำระเงิน</a>
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/payment" class="block py-4 mb-3 px-4 text-xl">แดชบอร์ด</a>
        <hr class="border-white mx-4 w-32" />
      </div>
    </div>
  </div>

  <div id="rest" class="flex-1 my-5">
    {@render children()}
  </div>
</div>

<style>
  .sidebar {
    min-width: 12rem;
  }
</style>
