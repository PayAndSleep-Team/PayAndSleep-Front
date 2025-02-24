<script>
    /** @type {{ data: import('./$types').LayoutData, children: import('svelte').Snippet }} */
    let { data, children } = $props();

    import { goto } from "$app/navigation";
    import { onMount } from "svelte";

    const Api_url = "http://localhost:3000";
    let user = $state({});

    onMount(async () => {
        try {
            const res = await fetch(`${Api_url}/api/session`, {
                method: "GET",
                credentials: "include",
            });
            user = await res.json();
            if (user.role !== "host") {
                goto("/login");
            }
        } catch (error) {
            console.error("Error fetching message:", error);
            message = "Failed to load message";
        }
    });
</script>

{@render children()}
