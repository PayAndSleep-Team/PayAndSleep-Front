<script>
    import { onMount, onDestroy } from "svelte";
    import { page } from "$app/stores";

    const Api_url = "http://localhost:3000";

    let tempRoom = "45/96";
    let tempBuilding = "ตึก A";
    let tempMessage = "จะมีการปรับปรุงพื้นสระว่ายนํ้า ในวันที่ 17/2/67";
    let tempRequest = "ไม่พบคำร้องซ่อมบำรุง";

    onMount(async () => {
        const unsubscribe = page.subscribe(($page) => {
            // You can add logic here if you need to fetch data based on query parameters
        });

        onDestroy(unsubscribe);

        // Simulated data fetching (remove this if you have actual API calls)
        // In a real scenario, you'd fetch data and update tempRoom, tempBuilding, tempMessage, tempRequest
    });
</script>

<div class="w-full relative flex flex-col items-center">
    <div class="relative w-[95%]">
        <img src="/images/mrbg.svg" class="w-full rounded-lg" alt="roompic" />
        <div class="ml-6 absolute bottom-4 left-6 text-white">
            <p class="text-4xl font-bold pb-4">{tempRoom}</p>
            <p class="text-xl font-medium">{tempBuilding}</p>
        </div>
    </div>
    <div class="w-full relative flex flex-col items-center pt-8">
        <div class="bg-[#8B8B8C] w-[95%] rounded-lg p-4 text-white flex items-center">
            <img src="/images/bell.svg" alt="Bell" class="w-6 h-6 mr-2" />
            <p class="text-xl font-normal text-[#404040]">{tempMessage}</p>
        </div>
        <div class="w-[95%] flex justify-start mt-6">
            <div class="bg-[#F2F2F2] text-black p-4 rounded-lg shadow flex-shrink-0 flex flex-col justify-center items-center w-[350px] h-[250px]">
                <p class="text-xl font-bold">คำร้องซ่อมบำรุง</p>
                <p class="text-xl text-[#8B8B8C] mb-3 mt-3">ไม่พบคำขอร้องซ่อมบำรุง</p>
                <button class="px-6 py-2 rounded-md text-xl mt-2 inline-block bg-[#557B55] text-white">ดำเนินการ</button>
            </div>
        </div>
    </div>
</div>