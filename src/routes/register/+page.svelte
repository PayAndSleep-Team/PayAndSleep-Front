<script>
    import { goto } from "$app/navigation";

    const Api_url = "http://localhost:3000";
    let lst = [];
    let message = $state("loading...");
    let email = $state("");
    let name = $state("");
    let phone = $state("");
    let role = $state("");
    let password = $state("");
    let confirm_password = $state("");

    async function register() {
        if (password !== confirm_password) {
            alert("Password and Confirm Password not match");
            return;
        }
        if (email === "" || password === "" || confirm_password === "" || role === "" || name === "" || phone === "") {
            alert("Please fill all fields");
            return;
        }
        console.log(email, password, role, name, phone);
        try {
            const res = await fetch(`${Api_url}/api/register`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    email: email,
                    password: password,
                    role: role,
                    name: name,
                    phone: phone
                }),
            });
            const data = await res.json();
            alert(data.message);
    
            if (data.message === "ลงทะเบียนสำเร็จ") {
                goto("/login");
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    }
</script>

<div class="w-full h-screen flex flex-col items-center justify-center">
    <!-- register form -->
    <div class="w-1/3 bg-white p-8 rounded-lg shadow-lg">
        <h1 class="text-2xl font-bold text-center">Register</h1>
        <form class="mt-4">
            <div class="mb-4">
                <label for="email" class="block text-gray-700 text-sm font-bold mb-2">Email</label>
                <input type="email" id="email" name="email" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={email} />
            </div>
            <div class="mb-4">
                <label for="name" class="block text-gray-700 text-sm font-bold mb-2">Name</label>
                <input type="text" id="name" name="name" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={name} />
            </div>
            <div class="mb-4">
                <label for="phone" class="block text-gray-700 text-sm font-bold mb-2">Phone</label>
                <input type="text" id="phone" name="phone" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={phone} />
            </div>
            <div class="mb-4">
                <label for="role" class="block text-gray-700 text-sm font-bold mb-2">Role</label>
                <select id="role" name="role" class="shadow border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={role}>
                    <option value="host">Host</option>
                    <option value="tenant">Tenant</option>
                </select>
            </div>
            <div class="mb-4">
                <label for="password" class="block text-gray-700 text-sm font-bold mb-2">Password</label>
                <input type="password" id="password" name="password" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={password} />
            </div>
            <div class="mb-4">
                <label for="confirm_password" class="block text-gray-700 text-sm font-bold mb-2">Confirm Password</label>
                <input type="password" id="confirm_password" name="confirm_password" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value={confirm_password} />
            </div>
            <div class="flex justify-between items-center">
                <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" onclick={() => register()}>Register</button>
                <a href="/login" class="text-indigo-500">Sign in</a>
            </div>
        </form>
    </div>
</div>
