<script>
    import { onMount } from 'svelte';

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
            const res = await fetch(`http://localhost:3000/api/get/notifications`, {
                method: "GET",
                credentials: "include",
            });
            notifications = await res.json();
        } catch (error) {
            console.error("Error fetching message:", error);
            notifications = [{ message: "Failed to load notifications"}];
        }
    });
</script>

<div class="">
    <div class="flex items-center" style="margin: 2rem;">
        <img src="/images/whiteRect.svg" alt="whiteRect">
        <p class="text-2xl ml-2 text-[#F2F2F2]">การแจ้งเตือน</p>
    </div>
    <div class="flex flex-col gap-6 mx-20">
        {#each notifications as notification}
            {#if notification.status === "read"}
                <!-- svelte-ignore a11y_click_events_have_key_events -->
                <!-- svelte-ignore a11y_no_static_element_interactions -->
                <div class="flex items-center bg-[#F2F2F2] rounded-lg shadow-md p-4">
                    <img src="/images/bell.svg" alt="bell" class="w-10 h-10 mr-4"/>
                    <p class="text-lg text-[#404040]">{notification.message}</p>
                </div>
            {:else}
                <!-- svelte-ignore a11y_click_events_have_key_events -->
                <!-- svelte-ignore a11y_no_static_element_interactions -->
                <div class="flex items-center bg-[#F2F2F2] rounded-lg shadow-md p-4 cursor-pointer" onclick={() => markAsRead(notification.id)}>
                    <img src="/images/redBell.svg" alt="bell" class="w-10 h-10 mr-4" />
                    <p class="text-lg text-[#404040]">{notification.message}</p>
                </div>
            {/if}
        {/each}
    </div>
</div>
