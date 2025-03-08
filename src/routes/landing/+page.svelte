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
</script> -->

