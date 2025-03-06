<script>
    import { fade, slide } from 'svelte/transition';
    
    export let value = "เลือกวิธีการชำระเงิน";
    let isOpen = false;
  
    const options = [
        { id: 'bank', label: 'บัญชีธนาคาร' },
        { id: 'promptpay', label: 'พร้อมเพย์' }
    ];

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
        class="w-full flex items-center justify-between px-6 py-3 text-white text-opacity-80 bg-white bg-opacity-10 
               rounded-3xl shadow-sm focus:outline-none focus:ring-2 focus:ring-white focus:ring-opacity-50 
               transition-all duration-300 hover:bg-opacity-20"
        on:click={toggleDropdown}
    >
        <p class="text-lg">{value}</p>
        <svg
            class="w-5 h-5 transition-transform duration-300 {isOpen ? 'rotate-180' : ''}"
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
        <!-- svelte-ignore a11y_interactive_supports_focus -->
        <div
            class="absolute z-10 w-full mt-2 bg-white bg-opacity-10 backdrop-blur-sm 
                   border border-white border-opacity-20 rounded-xl shadow-lg overflow-hidden"
            transition:slide={{ duration: 200 }}
        >
            <ul class="py-1">
                <!-- svelte-ignore a11y_no_noninteractive_element_interactions -->
                {#each options as option}
                    <!-- svelte-ignore a11y_click_events_have_key_events -->
                    <li
                        class="px-6 py-3 text-lg text-white text-opacity-80 hover:bg-white hover:bg-opacity-10 
                               cursor-pointer transition-colors duration-200"
                        on:click={() => selectOption(option.label)}
                    >
                        {option.label}
                    </li>
                {/each}
            </ul>
        </div>
    {/if}
</div>
  