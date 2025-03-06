<script>
    import { onMount } from 'svelte';
    import { fade, fly, slide } from 'svelte/transition';

    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();

    let notifications = $state([]);

    async function markAsRead(id) {
        try {
            const res = await fetch(`http://localhost:3000/api/read/notification`, {
                method: "PUT",
                credentials: "include",
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ id }),
            });
            const data = await res.json();
            if (data.message === "อัพเดทสถานะการแจ้งเตือนสำเร็จ") {
                notifications = notifications.map((notification) => {
                    if (notification.id === id) {
                        return { ...notification, status: "read" };
                    }
                    return notification;
                });
            }
        } catch (error) {
            console.error("Error fetching message:", error);
        }
    }

    onMount(async () => {
        try {
            notifications = await res.json();
        } catch (error) {
            console.error("Error fetching message:", error);
            notifications = [{ message: "Failed to load notifications"}];
        }
    });
</script>

<div class="min-h-screen" in:fade={{ duration: 300 }}>
    <div class="flex items-center px-4 sm:px-8 md:px-12 py-6 md:py-8" 
         in:fly={{ y: -20, duration: 600 }}>
        <img src="/images/whiteRect.svg" alt="whiteRect" 
             class="w-6 h-6 md:w-8 md:h-8 transition-transform hover:scale-110">
        <p class="text-xl md:text-2xl ml-2 text-[#F2F2F2]">การแจ้งเตือน</p>
    </div>
    <div class="flex flex-col gap-4 md:gap-6 px-4 sm:px-8 md:px-12 lg:px-20">
        {#each notifications as notification, i}
            <!-- svelte-ignore a11y_click_events_have_key_events -->
            <!-- svelte-ignore a11y_no_static_element_interactions -->
            <div 
                in:fly={{ y: 20, duration: 500, delay: i * 100 }}
                class="flex items-center bg-[#F2F2F2] rounded-lg shadow-md p-4 
                       transform transition-all duration-300 hover:scale-[1.02] hover:shadow-lg
                       cursor-pointer"
                onclick={() => markAsRead(notification.id)}
            >
                {#if notification.status === "read"}
                    <img src="/images/bell.svg" alt="bell" 
                         class="w-8 h-8 md:w-10 md:h-10 mr-3 md:mr-4 transition-transform hover:rotate-12"/>
                {:else}
                    <div class="relative">
                        <img src="/images/redBell.svg" alt="bell" 
                             class="w-8 h-8 md:w-10 md:h-10 mr-3 md:mr-4 transition-transform hover:rotate-12"/>
                        <span class="absolute top-0 right-0 h-3 w-3 bg-red-500 rounded-full"></span>
                    </div>
                {/if}
                <p class="text-base md:text-lg text-[#404040] flex-1">
                    {notification.message}
                </p>
            </div>
        {:else}
            <div 
                class="text-center text-[#F2F2F2] text-lg md:text-xl p-8"
                in:fade
            >
                ไม่มีการแจ้งเตือน
            </div>
        {/each}
    </div>
</div>