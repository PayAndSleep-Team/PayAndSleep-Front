<script>
  import "../../app.css";
  import { fade, fly, slide } from 'svelte/transition';
  let { children } = $props();
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  const Api_url = "http://localhost:3000";
  let user = $state({});
  let isSidebarOpen = $state(false);

  const toggleSidebar = () => {
    isSidebarOpen = !isSidebarOpen;
  };

  onMount(async () => {
    try {
      const res = await fetch(`${Api_url}/api/session`, {
        method: "GET",
        credentials: "include",
      });
      user = await res.json();
      if (user.role !== "host") {
        goto("/landing/login");
      }
    } catch (error) {
      console.error("Error fetching message:", error);
      message = "Failed to load message";
    }
  });
</script>

<div class="flex min-h-screen overflow-hidden">
  <!-- Mobile Menu Button -->
  <button 
    class="md:hidden fixed top-4 left-4 z-50 bg-[#404040] p-2 rounded-lg"
    onclick={toggleSidebar}
  >
    <img src="/images/menu.svg" alt="menu" class="w-6 h-6" />
  </button>

  <!-- Sidebar -->
  <div
    class="sidebar fixed md:relative bg-[#404040] text-white py-6 md:py-8 my-5 mx-2 md:mx-4 
           flex flex-col rounded-2xl items-center justify-center
           {isSidebarOpen ? 'translate-x-0' : '-translate-x-full'}
           md:translate-x-0 transition-transform duration-300 ease-in-out
           z-40 md:z-auto"
    in:fly={{ x: -20, duration: 600 }}
  >
    <nav class="space-y-1 md:space-y-2 w-full px-2">
      {#each [
        { href: '/host/', text: 'หน้าหลัก' },
        { href: '/host/userprofile', text: 'ข้อมูลส่วนตัว' },
        { href: '/host/notifications', text: 'การแจ้งเตือน' },
        { href: '/host/room', text: 'ห้องพัก' },
        { href: '/host/submitrequest', text: 'จัดการส่งคำร้อง\nช่องบำรุง' },
        { href: '/host/payment', text: 'จัดการสัญญา\nผู้เช่าใหม่' }
      ] as item, i}
        <div class="flex flex-col items-center mb-2 md:mb-4" 
             in:fly={{ x: -20, duration: 300, delay: i * 100 }}>
          <a
            href={item.href}
            class="block py-3 md:py-4 mb-1 md:mb-3 px-2 md:px-4 text-sm md:text-base lg:text-xl text-center
                   hover:bg-[#505050] rounded-lg transition-colors duration-200
                   whitespace-pre-line"
          >
            {item.text}
          </a>
          <hr class="border-white mx-2 md:mx-4 w-24 md:w-32 opacity-50" />
        </div>
      {/each}
    </nav>
  </div>

  <!-- Main Content -->
  <div
    id="rest"
    class="flex-1 my-5 px-2 sm:px-4 md:px-0 transition-all duration-300 overflow-x-hidden"
    in:fade={{ duration: 300 }}
  >
    {@render children()}
  </div>

  <!-- Overlay for mobile -->
  {#if isSidebarOpen}
    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div 
      class="fixed inset-0 bg-black bg-opacity-50 z-30 md:hidden"
      onclick={toggleSidebar}
      transition:fade={{ duration: 200 }}
    ></div>
  {/if}
</div>

<style>
  .sidebar {
    min-width: 10rem;
    max-width: 16rem;
    height: calc(100vh - 2.5rem);
  }

  @media (max-width: 768px) {
    .sidebar {
      position: fixed;
      left: 0;
      top: 0;
      height: 100vh;
      margin: 0;
      min-width: 8rem;
      max-width: 12rem;
    }
  }

  @media (max-width: 375px) {
    .sidebar {
      min-width: 7rem;
      max-width: 10rem;
    }
  }
</style>