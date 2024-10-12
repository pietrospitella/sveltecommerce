<script lang="ts">
	import { page } from "$app/stores"

	const props = $page.params
	console.log(props);
	
</script>

<div class="min-h-screen bg-background text-foreground">
	<main class="container mx-auto px-4 py-8">
		<h1 class="mb-6 text-3xl font-bold">Detail product {props.detail}</h1>
	</main>
</div>
