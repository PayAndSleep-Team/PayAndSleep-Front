<script>
	import "../app.css";
	import { fade } from 'svelte/transition';
	let { children } = $props();
</script>

<div class="bg-[#404040]">
	<div
		class="w-full min-h-screen bg-cover bg-center bg-no-repeat relative overflow-x-hidden"
		style="background-image: url('/images/bg.svg');"
	>
		{@render children()}
	</div>
</div>
