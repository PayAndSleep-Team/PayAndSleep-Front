<script>
    import { onMount } from 'svelte';

    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();

    let notifications = $state([]);

    onMount(async () => {
        try {
            const res = await fetch(`http://localhost:3000/api/get/notifications`, {
                method: "GET",
                credentials: "include",
            });
            notifications = await res.json();
        } catch (error) {
            console.error("Error fetching message:", error);
            notifications = ["Failed to load notifications"];
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
            <div class="flex items-center bg-[#D9D9D9] rounded-lg shadow-md p-4">
                <img src="/images/bell.svg" alt="bell" class="w-8 h-8 mr-4" />
                <p class="text-lg text-[#404040]">{notification.message}</p>
            </div>
        {/each}
    </div>
</div>
<!-- <div class="w-full h-full flex justify-center p-6">
        <div class="flex-grow overflow-y-auto space-y-6">
            {#each notifications as notification}
                <div class="ml-32 mr-32 flex h-[10%] items-center bg-[#D9D9D9] rounded-lg shadow-md p-4" style="background-color: #D9D9D9; margin-bottom: 43px;">
                    <img src="/images/bell.svg" alt="bell" class="w-6 h-6 mr-4" />
                    <p class="text-lg font-normal" style="color: #404040; margin-left: 1rem;">{notification}</p>
                </div>
            {/each}
        </div>
    </div>
</div> -->
