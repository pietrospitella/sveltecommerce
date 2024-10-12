<script lang="ts">
	import { Button } from '$lib/components/ui/button'
	import { Input } from '$lib/components/ui/input'
	import { Label } from '$lib/components/ui/label'
	import { Checkbox } from '$lib/components/ui/checkbox'
	import { LayoutGrid, List, X } from 'lucide-svelte'

	let isGridView = true
	let activeFilters: string[] = []
	let searchTerm = ''

	const categories = ['Electronics', 'Clothing', 'Books', 'Home & Garden']
	const products = [
		{ id: 1, name: 'Smartphone', category: 'Electronics', price: 599 },
		{ id: 2, name: 'T-shirt', category: 'Clothing', price: 19.99 },
		{ id: 3, name: 'Novel', category: 'Books', price: 12.99 },
		{ id: 4, name: 'Plant Pot', category: 'Home & Garden', price: 24.99 },
		{ id: 5, name: 'Laptop', category: 'Electronics', price: 999 },
		{ id: 6, name: 'Jeans', category: 'Clothing', price: 49.99 }
	]

	$: filteredProducts = products.filter(
		(product) =>
			(activeFilters.length === 0 || activeFilters.includes(product.category)) &&
			product.name.toLowerCase().includes(searchTerm.toLowerCase())
	)

	function toggleFilter(category: string) {
		activeFilters = activeFilters.includes(category)
			? activeFilters.filter((f) => f !== category)
			: [...activeFilters, category]
	}

	function clearFilter(filter: string) {
		activeFilters = activeFilters.filter((f) => f !== filter)
	}
</script>

<div class="min-h-screen bg-background text-foreground">
	<main class="container mx-auto px-4 py-8">
		<h1 class="mb-6 text-3xl font-bold">Products</h1>

		<div class="mb-6 flex flex-col gap-4 md:flex-row">
			<div class="w-full md:w-1/4">
				<Input type="text" placeholder="Search products..." bind:value={searchTerm} />
			</div>
			<div class="flex-grow">
				<div class="flex flex-wrap gap-2">
					{#each activeFilters as filter}
						<Button variant="outline" size="sm" on:click={() => clearFilter(filter)}>
							{filter}
							<X class="ml-2 h-4 w-4" />
						</Button>
					{/each}
				</div>
			</div>
			<div class="flex gap-2">
				<Button variant={isGridView ? 'default' : 'outline'} on:click={() => (isGridView = true)}>
					<LayoutGrid class="mr-2 h-4 w-4" /> Grid
				</Button>
				<Button variant={!isGridView ? 'default' : 'outline'} on:click={() => (isGridView = false)}>
					<List class="mr-2 h-4 w-4" /> List
				</Button>
			</div>
		</div>

		<div class="flex flex-col gap-6 md:flex-row">
			<div class="w-full md:w-1/4">
				<h2 class="mb-4 text-xl font-semibold">Filters</h2>
				{#each categories as category}
					<div class="mb-2 flex items-center space-x-2">
						<Checkbox
							id={category}
							checked={activeFilters.includes(category)}
							on:click={() => toggleFilter(category)}
						/>
						<Label for={category}>{category}</Label>
					</div>
				{/each}
			</div>

			<div class="flex-grow">
				{#if isGridView}
					<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
						{#each filteredProducts as product (product.id)}
							<div class="rounded-lg border p-4">
								<h3 class="text-lg font-semibold">{product.name}</h3>
								<p class="text-sm text-gray-600">{product.category}</p>
								<p class="mt-2 text-lg font-bold">${product.price.toFixed(2)}</p>
							</div>
						{/each}
					</div>
				{:else}
					<div class="space-y-4">
						{#each filteredProducts as product (product.id)}
							<div class="flex items-center justify-between rounded-lg border p-4">
								<div>
									<h3 class="text-lg font-semibold">{product.name}</h3>
									<p class="text-sm text-gray-600">{product.category}</p>
								</div>
								<p class="text-lg font-bold">${product.price.toFixed(2)}</p>
							</div>
						{/each}
					</div>
				{/if}
			</div>
		</div>
	</main>
</div>
