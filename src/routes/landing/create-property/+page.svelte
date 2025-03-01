<script>
    import { goto } from "$app/navigation";

    const Api_url = "http://localhost:3000";

    let name = $state("");
    let address = $state("");
    let water_rate = $state(0);
    let electricity_rate = $state(0);
    let bank_name = $state("");
    let bank_account_number = $state("");
    let account_holder_name = $state("");
    let promptpay_number = $state("");

    async function createProperty() {
        try {
            const res = await fetch(`${Api_url}/api/create/property`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    name: name,
                    address: address,
                    water_rate: water_rate,
                    electricity_rate: electricity_rate,
                    bank_name: bank_name,
                    bank_account_number: bank_account_number,
                    account_holder_name: account_holder_name,
                    promptpay_number: promptpay_number
                }),
                credentials: "include",
            });
            const data = await res.json();
            alert(data.message);

            if (data.message === "สร้าง Property สำเร็จ") {
                goto("/");
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    }
</script>

<div class="w-full h-screen flex flex-col items-center justify-center">
    <!-- create property -->
    <div class="w-1/3 bg-white p-8 rounded-lg shadow-lg">
        <h1 class="text-2xl font-bold text-center">Create Property</h1>
        <form class="mt-4">
            <div class="mb-4">
                <label for="name" class="block text-gray-700 text-sm font-bold mb-2">Name</label>
                <input type="text" id="name" name="name" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={name} />
            </div>
            <!-- address -->
            <div class="mb-4">
                <label for="address" class="block text-gray-700 text-sm font-bold mb-2">Address</label>
                <textarea id="address" name="address" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={address}></textarea>
            </div>
            <!-- water rate -->
            <div class="mb-4">
                <label for="water_rate" class="block text-gray-700 text-sm font-bold mb-2">Water Rate</label>
                <input type="number" id="water_rate" name="water_rate" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={water_rate} />
            </div>
            <!-- electric rate -->
            <div class="mb-4">
                <label for="electricity_rate" class="block text-gray-700 text-sm font-bold mb-2">Electric Rate</label>
                <input type="number" id="electricity_rate" name="electricity_rate" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={electricity_rate} />
            </div>
            <!-- bank name -->
            <div class="mb-4">
                <label for="bank_name" class="block text-gray-700 text-sm font-bold mb-2">Bank Name</label>
                <select id="bank_name" name="bank_name" class="shadow border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={bank_name}>
                    <option value="ธนาคารกรุงเทพ">ธนาคารกรุงเทพ</option>
                    <option value="ธนาคารกสิกรไทย">ธนาคารกสิกรไทย</option>
                    <option value="ธนาคารไทยพาณิชย์">ธนาคารไทยพาณิชย์</option>
                    <option value="ธนาคารกรุงไทย">ธนาคารกรุงไทย</option>
                    <option value="ธนาคารทหารไทย">ธนาคารทหารไทย</option>
                    <option value="ธนาคารยูโอบี">ธนาคารยูโอบี</option>
                </select>
            </div>
            <!-- bank account number -->
            <div class="mb-4">
                <label for="bank_account_number" class="block text-gray-700 text-sm font-bold mb-2">Bank Account Number</label>
                <input type="text" id="bank_account_number" name="bank_account_number" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={bank_account_number} />
            </div>
            <!-- account holder name -->
            <div class="mb-4">
                <label for="account_holder_name" class="block text-gray-700 text-sm font-bold mb-2">Account Holder Name</label>
                <input type="text" id="account_holder_name" name="account_holder_name" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={account_holder_name} />
            </div>
            <!-- promptpay number -->
            <div class="mb-4">
                <label for="promptpay_number" class="block text-gray-700 text-sm font-bold mb-2">Promptpay Number</label>
                <input type="text" id="promptpay_number" name="promptpay_number" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={promptpay_number} />
            </div>
            <!-- create property button -->
            <div class="flex justify-between items-center">
                <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" onclick={() => createProperty()}>Create Property</button>
                <a href="/" class="text-indigo-500">Back</a>
            </div>
        </form>
    </div>
</div>