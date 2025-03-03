<script>
    import { onMount } from 'svelte';
    import Chart from 'chart.js/auto';

    let availableRooms = 12;
    let occupiedRooms = 48;
    let paid = 40000;
    let unpaid = 15000;
    let totalBalance = paid + unpaid;
    let currentTenants = [
        { room: "406 - 21", name: "นายสมชาย ขาวสะอาด", time: "12/09/67 09:28 น." },
        { room: "406 - 10", name: "ห้องว่างรอการโอน", time: "12/09/67 09:28 น." }
    ];

    let unpaidBalances = [
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." }
    ];

    let chartDoughnut;

    onMount(() => {
        const dataDoughnut = {
            labels: ["ชำระแล้ว", "ยังไม่ชำระ"],
            datasets: [
                {
                    label: "ห้องพัก",
                    data: [paid, unpaid],
                    backgroundColor: [
                        "rgb(85, 123, 85)",
                        "rgb(208, 76, 76)",
                    ],
                    hoverOffset: 4,
                },
            ],
        };

        const innerText = {
            id: 'innerText',
            afterDraw(chart, args, options) {
                const { ctx, chartArea: { width, height } } = chart;
                const text1 = "ค่าเช่า";
                const text2 = totalBalance;
                const text3 = "บาท";

                ctx.save();
                ctx.textAlign = 'center';

                // Draw "ค่าเช่า"
                ctx.font = 'bold 16px sans-serif';
                ctx.fillStyle = 'black';
                ctx.fillText(text1, width / 2, height / 2 - 20);

                // Draw totalBalance (larger and bold)
                ctx.font = 'bold 24px sans-serif';
                ctx.fillText(text2, width / 2, height / 2 + 5);

                // Draw "บาท"
                ctx.font = 'bold 16px sans-serif';
                ctx.fillText(text3, width / 2, height / 2 + 30);

                ctx.restore();
            }
        };

        const configDoughnut = {
            type: "doughnut",
            data: dataDoughnut,
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'bottom',
                    },
                },
            },
            plugins: [innerText]
        };

        const ctx = document.getElementById('chartDoughnut').getContext('2d');
        chartDoughnut = new Chart(ctx, configDoughnut);
    });
</script>

<div class="w-full min-h-screen flex items-center justify-center p-6">
    <div class="w-[90%] h-[90%] p-10">
        <h2 class="text-2xl font-bold mb-6 text-left text-[#F2F2F2]">จัดการห้องพัก</h2>

        <div class="grid grid-cols-2 gap-6 mb-6">
            <div class="pl-8 bg-gray-200 p-4 rounded-lg text-center relative">
                <p class="text-xl text-[#D0854C] text-left font-bold">ห้องว่าง</p>
                <p class="text-4xl text-left text-[#D0854C] font-bold">{availableRooms}</p>
                <p class="text-xl text-[#D0854C] text-left font-bold">ห้อง</p>
            </div>
            <div class="pl-8 bg-gray-200 p-4 rounded-lg text-center">
                <p class="text-xl text-[#546882] text-left font-bold">ห้องที่มีผู้เข้า</p>
                <p class="text-4xl text-left text-[#546882] font-bold">{occupiedRooms}</p>
                <p class="text-xl text-[#546882] text-left font-bold">ห้อง</p>
            </div>
        </div>

        <div class="grid grid-cols-3 gap-6 mb-6">
            <div class="bg-white p-6 rounded-lg shadow-md relative">
                <div class="w-[90%] h-[100%] relative mx-auto max-w-[400px] min-w-[200px]">
                    <canvas id="chartDoughnut" class="z-50"></canvas>
                </div>
            </div>

            <div class="col-span-2 bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4">รายการผู้เช่าปัจจุบัน</h3>
                {#each currentTenants as tenant}
                    <div class="flex justify-between p-3 border-b">
                        <div class="flex"><p class="text-lg font-bold">{tenant.room}</p>
                            <p class="text-lg">&nbsp; - {tenant.name}</p></div>
                        <p class="text-gray-500">{tenant.time}</p>
                    </div>
                {/each}
            </div>
        </div>

        <div class="grid grid-cols-2 gap-6">
            <div class="bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4 text-green-600">ยอดรายการที่ยังไม่ชำระ</h3>
                {#each unpaidBalances as balance}
                    <div class="flex justify-between p-3 border-b text-green-600">
                        <p class="font-bold">{balance.room}</p>
                        <p>{balance.amount} บาท</p>
                        <p>{balance.dueDate}</p>
                    </div>
                {/each}
            </div>
            <div class="bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4 text-red-600">ยอดรายการที่ยังไม่ชำระ</h3>
                {#each unpaidBalances as balance}
                    <div class="flex justify-between p-3 border-b text-red-600">
                        <p class="font-bold">{balance.room}</p>
                        <p>{balance.amount} บาท</p>
                        <p>{balance.dueDate}</p>
                    </div>
                {/each}
            </div>
        </div>
    </div>
</div>