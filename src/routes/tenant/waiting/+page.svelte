<script>
    import { fade, fly, scale } from 'svelte/transition';
    /** @type {{ data: import('./$types').PageData }} */
    let { data } = $props();
</script>

<div class="grid grid-cols-1 md:grid-cols-2 min-h-full" in:fade={{ duration: 300 }}>
    <div class="hidden md:flex items-center justify-center p-6">
    </div>
    <div 
        class="bg-[#f2f2f2] bg-opacity-80 rounded-xl m-4 md:m-8 p-8 flex flex-col justify-center items-center shadow-lg"
        in:fly={{ x: 20, duration: 600 }}
    >
        <div class="mb-8 animate-pulse" in:scale={{ duration: 400, delay: 300 }}>
            <img 
                src="/images/approvefile.svg" 
                alt="approvefile" 
                class="w-32 h-32 md:w-40 md:h-40"
            />
        </div>
        <h1 class="text-2xl md:text-3xl font-bold text-[#404040] mb-4" in:fly={{ y: 20, duration: 400, delay: 400 }}>
            อยู่ระหว่างการตรวจสอบ
        </h1>
    </div>
</div>