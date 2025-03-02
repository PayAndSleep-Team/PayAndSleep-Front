<script>
  import "../../app.css";
  let { children } = $props();
</script>

<div class="flex min-h-screen w-full">
  <div
    class="sidebar bg-[#404040] text-white flex flex-col rounded-2xl items-center justify-center w-[200px] m-5 absolute h-[96%]"
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

  <div
    id="rest"
    class="flex-1 bg-[#F2F2F2] bg-opacity-[0.85] m-5 p-4 rounded-2xl pl-[200px]"
  >
    {@render children()}
  </div>
</div>



<style>
  .sidebar {
    min-width: 12rem;
  }
</style>
