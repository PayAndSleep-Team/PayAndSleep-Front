<script>
    import { goto } from "$app/navigation";

    const Api_url = "http://localhost:3000";
    let email = "";
    let password = "";
    let check = false;

    async function login() {
        try {
            const res = await fetch(`${Api_url}/api/login`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    email: email,
                    password: password,
                }),
                credentials: "include",
            });
            const data = await res.json();
            alert(data.message);

            if (data.message === "เข้าสู่ระบบสำเร็จ") {
                goto("/");
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    }

    const showPopup = () => {
        check = true;
    };
</script>

<div class="w-full h-screen flex flex-col items-center justify-center">
    {#if !check}
        <div class="w-96 bg-white p-8 rounded-lg shadow-lg">
            <h1 class="text-2xl font-bold text-center">Login</h1>
            <form class="mt-4">
                <div class="mb-4">
                    <label
                        for="email"
                        class="block text-sm font-medium text-gray-700"
                        >Email</label
                    >
                    <input
                        type="email"
                        id="email"
                        name="email"
                        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                        bind:value={email}
                    />
                </div>
                <div class="mb-4">
                    <label
                        for="password"
                        class="block text-sm font-medium text-gray-700"
                        >Password</label
                    >
                    <input
                        type="password"
                        id="password"
                        name="password"
                        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
                        bind:value={password}
                    />
                </div>
                <div class="flex justify-between items-center">
                    <button
                        type="submit"
                        class="bg-indigo-500 text-white px-4 py-2 rounded-md"
                        onclick={login}>Login</button
                    >
                    <button class="text-indigo-500" onclick="{showPopup}">
                        Sign up
                    </button>
                </div>
            </form>
        </div>
    {:else}
    <!-- ask Are u host or tenant -->
        <div class="w-96 bg-white p-8 rounded-lg shadow-lg">
            <h1 class="text-2xl font-bold text-center">คุณต้องการลงทะเบียนในสถานะใด</h1>
            <div class="flex justify-between items-center">
                <button
                    type="submit"
                    class="bg-indigo-500 text-white px-4 py-2 rounded-md"
                    onclick={() => goto("/register?role=host")}>Host</button
                >
                <button
                    type="submit"
                    class="bg-indigo-500 text-white px-4 py-2 rounded-md"
                    onclick={() => goto("/register?role=tenant")}>Tenant</button
                >
            </div>
        </div>
    {/if}
</div>
