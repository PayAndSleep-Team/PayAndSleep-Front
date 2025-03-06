<script>
    import { onMount } from "svelte";
    import Chart from "chart.js/auto";

    const Api_url = "http://localhost:3000";

    let availableRooms = 0;
    let occupiedRooms = 0;
    let paid = 0;
    let unpaid = 0;
    let totalBalance;
    let currentTenants = [
        {
            room: "406 - 21",
            name: "นายสมชาย ขาวสะอาด",
            time: "12/09/67 09:28 น.",
        },
        {
            room: "406 - 10",
            name: "ห้องว่างรอการโอน",
            time: "12/09/67 09:28 น.",
        },
        {
            room: "406 - 21",
            name: "นายสมชาย ขาวสะอาด",
            time: "12/09/67 09:28 น.",
        },
        {
            room: "406 - 21",
            name: "นายสมชาย ขาวสะอาด",
            time: "12/09/67 09:28 น.",
        },
        {
            room: "406 - 21",
            name: "นายสมชาย ขาวสะอาด",
            time: "12/09/67 09:28 น.",
        },
        {
            room: "406 - 21",
            name: "นายสมชาย ขาวสะอาด",
            time: "12/09/67 09:28 น.",
        },
        {
            room: "406 - 21",
            name: "นายสมชาย ขาวสะอาด",
            time: "12/09/67 09:28 น.",
        },
    ];

    let unpaidBalances = [
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 29", amount: 4678, dueDate: "10/09/67 17:51 น." },
    ];
    let paidBalances = [
        { room: "406 - 12", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 12", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 12", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 12", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 12", amount: 4678, dueDate: "10/09/67 17:51 น." },
        { room: "406 - 12", amount: 4678, dueDate: "10/09/67 17:51 น." },
    ];
    let chartDoughnut;

    onMount(async () => {
        try {
            const res = await fetch(`${Api_url}/api/get/rooms`, {
                method: "GET",
                credentials: "include",
            });
            const data = await res.json();
            data.forEach((item) => {
                if (item.status === "available") {
                    availableRooms++;
                } else {
                    occupiedRooms++;
                }
            });

            const res2 = await fetch(`${Api_url}/api/get/rental-contracts`, {
                method: "GET",
                credentials: "include",
            });
            const data2 = await res2.json();
            currentTenants = data2.filter((item) => item.status === "active").map((item) => {
                return {
                    room: item.room_number,
                    name: item.tenant_name,
                    time: item.created_at,
                };
            });

            const res3 = await fetch(`${Api_url}/api/get/bills`, {
                method: "GET",
                credentials: "include",
            });
            const data3 = await res3.json();
            console.log(data3);
            unpaidBalances = data3
                .filter((item) => item.status === "pending")
                .map((item) => {
                    return {
                        room: item.room_number,
                        amount: item.total_amount,
                        dueDate: item.month_year,
                    };
                });
            unpaid = unpaidBalances.reduce((acc, item) => acc + item.amount, 0);
            paidBalances = data3
                .filter((item) => item.status === "paid")
                .map((item) => {
                    return {
                        room: item.room_number,
                        amount: item.total_amount,
                        dueDate: item.month_year,
                    };
                });
            paid = paidBalances.reduce((acc, item) => acc + item.amount, 0);
            totalBalance = paid + unpaid;
        } catch (error) {
            console.error("Error fetching message:", error);
        }

        const dataDoughnut = {
            labels: ["ชำระแล้ว", "ยังไม่ชำระ"],
            datasets: [
                {
                    label: "ห้องพัก",
                    data: [paid, unpaid],
                    backgroundColor: ["rgb(85, 123, 85)", "rgb(208, 76, 76)"],
                    hoverOffset: 4,
                },
            ],
        };

        const innerText = {
            id: "innerText",
            afterDraw(chart, args, options) {
                const {
                    ctx,
                    chartArea: { width, height },
                } = chart;
                const text1 = "ค่าเช่า";
                const text2 = totalBalance;
                const text3 = "บาท";

                ctx.save();
                ctx.textAlign = "center";

                // Draw "ค่าเช่า"
                ctx.font = "bold 16px sans-serif";
                ctx.fillStyle = "black";
                ctx.fillText(text1, width / 2, height / 2 - 20);

                // Draw totalBalance (larger and bold)
                ctx.font = "bold 24px sans-serif";
                ctx.fillText(text2, width / 2, height / 2 + 5);

                // Draw "บาท"
                ctx.font = "bold 16px sans-serif";
                ctx.fillText(text3, width / 2, height / 2 + 30);

                ctx.restore();
            },
        };

        const configDoughnut = {
            type: "doughnut",
            data: dataDoughnut,
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: "bottom",
                    },
                },
            },
            plugins: [innerText],
        };

        const ctx = document.getElementById("chartDoughnut").getContext("2d");
        chartDoughnut = new Chart(ctx, configDoughnut);
    });
</script>

<div class="w-full min-h-screen flex items-center justify-center p-4">
    <div class="w-full">
        <h2 class="text-2xl font-bold mb-6 text-left text-[#F2F2F2]">
            จัดการห้องพัก
        </h2>

        <div class="grid grid-cols-2 gap-6 mb-6">
            <div class="bg-white rounded-xl shadow overflow-hidden p-4">
                <div class="flex">
                    <div><img src="/images/orgRect.svg" alt="" /></div>
                    <div class="flex flex-col">
                        <div class="px-3 text-xl text-[#D0854C]">
                            ห้องที่ว่าง
                        </div>
                        <div class="px-3 text-4xl font-bold text-[#D0854C]">
                            {availableRooms}
                        </div>
                        <div class="px-3 text-lg text-[#D0854C]">ห้อง</div>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-xl shadow overflow-hidden p-4">
                <div class="flex">
                    <div><img src="/images/blueRect.svg" alt="" /></div>
                    <div class="flex flex-col">
                        <div class="px-3 text-xl text-[#546882]">
                            ห้องที่ว่าง
                        </div>
                        <div class="px-3 text-4xl font-bold text-[#546882]">
                            {occupiedRooms}
                        </div>
                        <div class="px-3 text-lg text-[#546882]">ห้อง</div>
                    </div>
                </div>
            </div>
        </div>

        <div class="grid grid-cols-3 gap-6 mb-6">
            <div
                class="bg-white p-6 rounded-lg shadow-md relative flex justify-center items-center"
            >
                <div class="relative mx-auto">
                    <canvas id="chartDoughnut" class="z-50"></canvas>
                </div>
            </div>

            <div class="col-span-2 bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4">รายการผู้เช่าปัจจุบัน</h3>
                <div class="h-72 overflow-y-auto">
                    {#each currentTenants as tenant}
                        <div
                            class="flex justify-between items-center p-3 border-b"
                        >
                            <div class="flex">
                                <p class="text-lg font-bold">{tenant.room}</p>
                                <p class="text-lg pl-2">- {tenant.name}</p>
                            </div>
                            <p class="text-[#8B8B8C]">{tenant.time}</p>
                        </div>
                    {/each}
                </div>
            </div>
        </div>

        <div class="grid grid-cols-2 gap-6">
            <div class="bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4 text-[#557B55]">
                    ยอดรายการชำระแล้ว
                </h3>
                <div class="h-64 overflow-y-auto">
                    {#each paidBalances as balance}
                        <div
                            class="flex justify-between p-3 border-b text-[#557B55]"
                        >
                            <p class="font-bold">{balance.room}</p>
                            <p>{balance.amount} บาท</p>
                            <p>{balance.dueDate}</p>
                        </div>
                    {/each}
                </div>
            </div>

            <div class="bg-white p-6 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4 text-[#D04C4C]">
                    ยอดรายการที่ยังไม่ชำระ
                </h3>
                <div class="h-64 overflow-y-auto">
                    {#each unpaidBalances as balance}
                        <div
                            class="flex justify-between p-3 border-b text-[#D04C4C]"
                        >
                            <p class="font-bold">{balance.room}</p>
                            <p>{balance.amount} บาท</p>
                            <p>{balance.dueDate}</p>
                        </div>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>
