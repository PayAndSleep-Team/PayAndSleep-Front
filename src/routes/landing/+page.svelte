<!-- <script>
    import { goto } from "$app/navigation";
    const Api_url = "http://localhost:3000";
    let message = $state("loading...");
    let user = $state({});
    let profile = $state({});
    import { onMount } from "svelte"; 

    onMount(async () => {
        try {
            const res = await fetch(`${Api_url}/api/session`, {
                method: "GET",
                credentials: "include",
            });
            user = await res.json();
            profile = user.profile;
            console.log($state.snapshot(user));
            if (user.message === "ไม่ได้เข้าสู่ระบบ") {
                goto("/login");
            }
            if (user.role === "host") {
                window.location.href = 'http://localhost:3000/api/host/property';
            }
            if (user.role === "tenant") {
                window.location.href = 'http://localhost:3000/api/tenant/rental-contract';
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    });

    async function click() {
        try {
            const res = await fetch(`${Api_url}/api/eiei`);
            const eiei = await res.json();
            message = eiei.message;
            console.log(eiei);
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    }

    async function logout() {
        try {
            const res = await fetch(`${Api_url}/api/logout`, {
                method: "GET",
                credentials: "include",
            });
            const data = await res.json();
            alert(data.message);
            if (data.message === "ออกจากระบบสำเร็จ") {
                window.location.href = "/login";
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    }
</script>

<div class="w-full h-screen flex flex-col items-center justify-center">
    <div class="w-1/3 bg-white p-8 rounded-lg shadow-lg">
        <h1 class="text-2xl font-bold text-center">Profile</h1>
        <div class="mt-4">
            <div class="mb-4">
                <label for="username" class="block text-gray-700 text-sm font-bold mb-2">Username</label>
                <p type="text" id="username" name="username" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">{profile.name}</p>
            </div>
            <div class="mb-4">
                <label for="email" class="block text-gray-700 text-sm font-bold mb-2">Email</label>
                <p type="email" id="email" name="email" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">{user.email}</p>
            </div>
            <div class="mb-4">
                <label for="role" class="block text-gray-700 text-sm font-bold mb-2">Role</label>
                <p type="text" id="role" name="role" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">{user.role}</p>
            </div>
            <div class="flex justify-end items-center">
                <button class="bg-red-500 text-white p-2 rounded-xl" onclick={logout}> logout </button>
            </div>
        </div>
    </div>
</div> -->
