<script>
    let name = "ปทิตตา ดวงแก้ว"; 
    let email = "patittadua@gmail.com";
    let password = "Pramandanijjanukor2546";
    let history = [
        { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
        { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
        { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
        { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
        { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },
        { month: "มกราคม 2567", amount: "8,324 บาท", status: "ชำระแล้ว" },

    ];
</script>

<div class="w-full min-h-screen bg-cover bg-center bg-no-repeat" style="background-image: url('/images/bg.png');">
    <div class="w-full h-screen flex flex-col items-center justify-center">
        <div class="w-[85%] h-[90%] pt-16 flex flex-col items-center px-10">
            <img src="/images/Icon.svg" alt="icon" class="mb-6 w-24 h-24 fill-[#F2F2F2]" />
            <p class="text-[#F2F2F2] font-normal font-jeju text-4xl">{name}</p>
            <p class="text-[#F2F2F2] font-normal font-jeju text-2xl mb-10">{email}</p>

            <div class="w-full max-w-2xl text-left mb-10">
                <p class="text-[#F2F2F2] text-2xl font-normal leading-none">รหัสผ่าน</p>
                <p class="text-[#F2F2F2] text-xl mt-2">{password}</p>
            </div>

            <div class="w-full max-w-2xl border-t border-gray-400 mb-8"></div>

            <div class="w-full max-w-3xl">
                <p class="text-[#F2F2F2] text-2xl font-normal leading-none mb-10">ประวัติการเช่า</p>
                <div class="grid grid-cols-2 gap-6">
                    {#each history as item}
                    <div class="bg-gray-200 border-[#404040] shadow-md rounded-lg p-4 border-l-4 pl-4">
                        <p class="text-black text-xl font-medium">{item.month} ยอดรวม {item.amount}</p>
                        <p class="text-gray-600 text-lg">สถานะ: {item.status}</p>
                    </div>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>
