<script>
	import "../app.css";
	let { children } = $props();
</script>

<div class="bg-[#404040]">
	<div
		class="w-full h-screen bg-cover bg-center bg-no-repeat relative"
		style="background-image: url('/images/bg.svg');"
	>
		{@render children()}
	</div>
</div>
