<script>
    import { onMount } from "svelte";
    import Chart from "chart.js/auto";
    import { fade, fly, scale } from "svelte/transition";

    const Api_url = "http://localhost:3000";

    let availableRooms = 0;
    let occupiedRooms = 0;
    let paid = 0;
    let unpaid = 0;
    let totalBalance;
    let currentTenants = [];

    let unpaidBalances = [];
    let paidBalances = [];
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
                    // time: item.start_date.split(" ")[0],
                    time: item.start_date,
                };
            });

            const res3 = await fetch(`${Api_url}/api/get/bills`, {
                method: "GET",
                credentials: "include",
            });
            const data3 = await res3.json();
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
            availableRooms = availableRooms || 0;
            occupiedRooms = occupiedRooms || 0;
            paid = paid || 0;
            unpaid = unpaid || 0;
            totalBalance = paid + unpaid;
        }

        if (paid !== undefined || unpaid !== undefined) {
            totalBalance = totalBalance || 0;
            
            const dataDoughnut = {
                labels: ["ชำระแล้ว", "ยังไม่ชำระ"],
                datasets: [
                    {
                        label: "ห้องพัก",
                        data: [paid || 0, unpaid || 0],
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

        // Destroy existing chart if it exists to prevent duplicates
        if (chartDoughnut) {
            chartDoughnut.destroy();
        }

        // Delay chart creation slightly to ensure container is ready
        setTimeout(() => {
            const ctx = document.getElementById("chartDoughnut");
            if (ctx) {
                chartDoughnut = new Chart(ctx.getContext("2d"), configDoughnut);
            }
        }, 100);
        }
    });
</script>

<div class="w-full min-h-screen flex items-center justify-center p-4">
    <div class="w-full max-w-7xl mx-auto">
        <h2 class="text-2xl md:text-3xl font-bold mb-6 text-left text-[#F2F2F2]" in:fade={{ duration: 400, delay: 100 }}>
            แดชบอร์ด
        </h2>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6 mb-6">
            <div class="bg-white rounded-2xl shadow overflow-hidden p-4 transform transition-all duration-300 hover:shadow-lg" 
                 in:fly={{ y: 20, duration: 400, delay: 200 }}>
                 <div class="flex items-center h-full">
                    <div class="h-full w-[11px] ml-1 my-2 bg-[#D0854C]"></div>
                    <div>
                      <div class="px-3 text-lg sm:text-xl text-[#D0854C]">ห้องว่างให้เช่า</div>
                      <div class="px-3 text-2xl sm:text-4xl font-bold text-[#D0854C]">
                        {availableRooms}
                      </div>
                      <div class="px-3 text-base sm:text-lg text-[#D0854C]">ห้อง</div>
                    </div>
                  </div>
            </div>
            <div class="bg-white rounded-2xl shadow overflow-hidden p-4 transform transition-all duration-300 hover:shadow-lg"
                 in:fly={{ y: 20, duration: 400, delay: 300 }}>
                 <div class="flex items-center h-full">
                    <div class="h-full w-[11px] ml-1 my-2 bg-[#546882]"></div>
                    <div>
                      <div class="px-3 text-lg sm:text-xl text-[#546882]">ห้องที่มีผู้เช่า</div>
                      <div class="px-3 text-2xl sm:text-4xl font-bold text-[#546882]">
                        {occupiedRooms}
                      </div>
                      <div class="px-3 text-base sm:text-lg text-[#546882]">ห้อง</div>
                    </div>
                  </div>
            </div>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 md:gap-6 mb-6">
            <div
                class="bg-white p-4 md:p-6 rounded-2xl shadow-md relative transform transition-all duration-300 hover:shadow-lg"
                in:scale={{ duration: 400, delay: 400, start: 0.8 }}
            >
                <div class="w-full h-full flex items-center justify-center py-2">
                    <canvas id="chartDoughnut" width="250" height="250"></canvas>
                </div>
            </div>

            <div class="lg:col-span-2 bg-white p-4 md:p-6 rounded-2xl shadow-md transform transition-all duration-300 hover:shadow-lg"
                 in:fly={{ x: 20, duration: 400, delay: 500 }}>
                <h3 class="text-lg md:text-xl font-bold mb-4">รายการผู้เช่าปัจจุบัน</h3>
                <div class="h-64 md:h-72 overflow-y-auto">
                    {#each currentTenants as tenant, i}
                        <div
                            class="flex flex-col sm:flex-row justify-between items-start sm:items-center p-3 border-b"
                            in:fade={{ duration: 300, delay: 600 + i * 50 }}
                        >
                            <div class="flex flex-col sm:flex-row sm:items-center mb-1 sm:mb-0">
                                <p class="text-base md:text-lg font-bold">{tenant.room}</p>
                                <p class="text-base md:text-lg sm:pl-2">- {tenant.name}</p>
                            </div>
                            <p class="text-sm md:text-base text-[#8B8B8C]">{tenant.time}</p>
                        </div>
                    {/each}
                </div>
            </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6">
            <div class="bg-white p-4 md:p-6 rounded-2xl shadow-md transform transition-all duration-300 hover:shadow-lg"
                 in:fly={{ y: 20, duration: 400, delay: 700 }}>
                <h3 class="text-lg md:text-xl font-bold mb-4 text-[#557B55]">
                    ยอดรายการชำระแล้ว
                </h3>
                <div class="h-56 md:h-64 overflow-y-auto">
                    {#each paidBalances as balance, i}
                        <div
                            class="flex flex-col sm:flex-row justify-between p-3 border-b text-[#557B55]"
                            in:fade={{ duration: 300, delay: 800 + i * 50 }}
                        >
                            <p class="font-bold mb-1 sm:mb-0">{balance.room}</p>
                            <div class="flex justify-between sm:justify-end w-full sm:w-auto">
                                <p class="sm:mr-4">{balance.amount} บาท</p>
                                <p>{balance.dueDate}</p>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>

            <div class="bg-white p-4 md:p-6 rounded-2xl shadow-md transform transition-all duration-300 hover:shadow-lg"
                 in:fly={{ y: 20, duration: 400, delay: 800 }}>
                <h3 class="text-lg md:text-xl font-bold mb-4 text-[#D04C4C]">
                    ยอดรายการที่ยังไม่ชำระ
                </h3>
                <div class="h-56 md:h-64 overflow-y-auto">
                    {#each unpaidBalances as balance, i}
                        <div
                            class="flex flex-col sm:flex-row justify-between p-3 border-b text-[#D04C4C]"
                            in:fade={{ duration: 300, delay: 900 + i * 50 }}
                        >
                            <p class="font-bold mb-1 sm:mb-0">{balance.room}</p>
                            <div class="flex justify-between sm:justify-end w-full sm:w-auto">
                                <p class="sm:mr-4">{balance.amount} บาท</p>
                                <p>{balance.dueDate}</p>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>
