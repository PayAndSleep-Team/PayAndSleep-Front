<script>
  import "../../app.css";
  let { children } = $props();
</script>

<div class="flex min-h-screen absolute">
  <div
    class="sidebar bg-[#404040] text-white py-8 m-5 flex flex-col rounded-2xl items-center justify-center w-1/12 "
  >
    <div>
      <div class="flex flex-col items-center mb-4">
        <a href="/" class="block py-4 mb-3 px-4 text-xl">หน้าหลัก</a>
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/profile" class="block py-4 mb-3 px-4 text-xl">ข้อมูลส่วนตัว</a>
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/notifications" class="block py-4 mb-3 px-4 text-xl">การแจ้งเตือน</a>
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/requests" class="block py-4 mb-3 px-4 text-xl">ส่งคำร้อง<br>ช่องบำรุง</a>
        <hr class="border-white mx-4 w-32" />
      </div>

      <div class="flex flex-col items-center mb-4">
        <a href="/payment" class="block py-4 mb-3 px-4 text-xl">ชำระเงิน</a>
        <hr class="border-white mx-4 w-32" />
      </div>
    </div>
  </div>
</div>

{@render children()}

<style>
  .sidebar {
    min-width: 12rem;
  }
</style>
