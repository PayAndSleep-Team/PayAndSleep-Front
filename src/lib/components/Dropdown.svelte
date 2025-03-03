<script>
    export let value = "เลือกวิธีการชำระเงิน";
    let isOpen = false;
  
    function toggleDropdown() {
      isOpen = !isOpen;
    }
  
    function selectOption(option) {
      value = option;
      isOpen = false;
    }
  </script>
  
  <div class="relative inline-block w-full">
    <button
      class="w-full flex items-center justify-between px-4 py-1 text-[#8B8B8C] border border-[#404040] rounded-full shadow-sm focus:outline-none"
      on:click={toggleDropdown}
    >
      <p class="mr-10">{value}</p>
      <svg
        class="w-4 h-4 ml-10 text-black"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M19 9l-7 7-7-7"
        />
      </svg>
    </button>
  
    {#if isOpen}
      <div
        class="absolute z-10 w-full mt-1 bg-white border border-gray-300 rounded-md shadow-lg"
      >
        <!-- svelte-ignore a11y_no_noninteractive_element_interactions -->
        <ul class="py-1">
          <!-- svelte-ignore a11y_click_events_have_key_events -->
          <li
            class="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 cursor-pointer"
            on:click={() => selectOption('บัญชีธนาคาร')}
          >
            บัญชีธนาคาร
          </li>
          <!-- svelte-ignore a11y_click_events_have_key_events -->
          <li
            class="px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 cursor-pointer"
            on:click={() => selectOption('พร้อมเพย์')}
          >
            พร้อมเพย์
          </li>
        </ul>
      </div>
    {/if}
  </div>
  