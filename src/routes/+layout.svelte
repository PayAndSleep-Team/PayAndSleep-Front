<script>
	import "../app.css";
	let { children } = $props();
</script>

<div class="bg-[#404040]">
	<div
		class="w-full h-full bg-cover bg-center bg-no-repeat relative bg-fixed"
		style="background-image: url('/images/bg.svg');"
	>
		{@render children()}
	</div>
</div>
