<script>
	import '../app.css';
	let { children } = $props();
</script>

<div>
	<nav class="bg-gray-800 p-4">
		<ul class="flex justify-between">
			<li class="flex items-center justify-center w-full">
				<a href="/" class="text-white">Home</a>
			</li>
			<li class="flex items-center justify-center w-full">
				<a href="/login" class="text-white">Login</a>
			</li>
			<li class="flex items-center justify-center w-full">
				<a href="/register" class="text-white">Register</a>
			</li>
		</ul>
	</nav>
</div>

{@render children()}
