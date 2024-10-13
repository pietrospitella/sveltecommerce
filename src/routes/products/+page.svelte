<script lang="ts">
	import { Button } from '$lib/components/ui/button'
	import { Input } from '$lib/components/ui/input'
	import { Label } from '$lib/components/ui/label'
	import { Checkbox } from '$lib/components/ui/checkbox'
	import { LayoutGrid, List, X } from 'lucide-svelte'
	import ProductCard from '$lib/components/ProductCard.svelte'
	import { onMount } from 'svelte'
	import { goto } from '$app/navigation'
	import { Skeleton } from '$lib/components/ui/skeleton'

	let isLoaded = false
	let isGridView = true
	let activeFilters: Filter[] = []

	interface IProduct {
		id: number
		title: string
		category: string
		description: string
		brand: string
		price: number
		rating: number
		reviews: any[]
		images: string[]
	}

	interface Filter {
		category: string
		options: string[]
		active: string[]
	}

	let filters: Filter[] = [
		{ category: 'Category', options: ['smartphones', 'laptops', 'tablets'], active: [] },
		{ category: 'Brand', options: ['Apple', 'Samsung', 'Oppo', 'Realme', 'Vivo'], active: [] },
		{
			category: 'Price',
			options: ['Under $50', '$50 - $100', '$100 - $200', 'Over $200'],
			active: []
		}
	]

	let products: IProduct[] = []

	function toggleFilter(category: string, option: string) {
		filters = filters.map((filter) => {
			if (filter.category === category) {
				if (filter.active.includes(option)) {
					filter.active = filter.active.filter((item) => item !== option)
				} else {
					filter.active = [...filter.active, option]
				}
			}
			return filter
		})
	}

	function clearFilter(category: string, option: string) {
		filters = filters.map((filter) => {
			if (filter.category === category) {
				filter.active = filter.active.filter((item) => item !== option)
			}
			return filter
		})
	}

	function clearAllFilters() {
		filters = filters.map((filter) => ({ ...filter, active: [] }))
	}

	$: activeFilters = filters.flatMap((filter) =>
		filter.active.map((option) => ({ category: filter.category, option }))
	)

	$: filteredProducts = products.filter((product) => {
		const matchesSearch = product.title.toLowerCase()
		const matchesFilters = filters.every((filter) => {
			if (filter.active.length === 0) return true
			switch (filter.category) {
				case 'Category':
					return filter.active.includes(product.category)
				case 'Brand':
					return filter.active.includes(product.brand)
				case 'Price':
					const priceRange = filter.active[0]
					if (priceRange === 'Under $50') return product.price < 50
					if (priceRange === '$50 - $100') return product.price >= 50 && product.price < 100
					if (priceRange === '$100 - $200') return product.price >= 100 && product.price < 200
					if (priceRange === 'Over $200') return product.price >= 200
					return true
				default:
					return true
			}
		})
		return matchesSearch && matchesFilters
	})

	onMount(() => {
		let response
		fetch('https://dummyjson.com/products/category/smartphones')
			.then((res) => (response = res.json()))
			.then((data) => {
				console.log(data)
				if (data) {
					products = data.products
					isLoaded = true
				}
			})
	})
</script>

<svelte:head>
	<title>All Products | SvelteCommerce</title>
</svelte:head>

<div class="min-h-screen bg-background text-foreground">
	<div class="container mx-auto p-4">
		<h1 class="mb-6 text-3xl font-bold">Products</h1>

		<div class="mb-6 flex flex-col gap-4 md:flex-row">
			<div class="flex-grow">
				<div class="flex flex-wrap gap-2">
					{#each activeFilters as filter}
						<Button
							variant="outline"
							size="sm"
							on:click={() => clearFilter(filter.category, filter.option)}
						>
							{filter.category}: {filter.option}
							<X class="ml-2 h-4 w-4" />
						</Button>
					{/each}
					{#if activeFilters.length > 0}
						<Button variant="outline" size="sm" on:click={clearAllFilters}>
							Clear All Filters
						</Button>
					{/if}
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
				<div class="flex md:flex-col justify-around">
					{#each filters as filter}
						<div class="mb-4 w-auto">
							<h3 class="mb-2 font-semibold">{filter.category}</h3>
							{#each filter.options as option}
								<div class="mb-2 flex items-center space-x-2">
									<Checkbox
										id={`${filter.category}-${option}`}
										checked={filter.active.includes(option)}
										on:click={() => toggleFilter(filter.category, option)}
									/>
									<Label for={`${filter.category}-${option}`} class="capitalize">{option}</Label>
								</div>
							{/each}
						</div>
					{/each}
				</div>
			</div>

			{#if isLoaded}
				<div class="flex-grow">
					{#if isGridView}
						<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
							{#each filteredProducts as product (product.id)}
								<ProductCard
									mode="grid"
									id={product.id}
									title={product.title}
									image={product.images[0]}
									category={product.category}
									price={product.price}
									brand={product.brand}
								/>
							{/each}
						</div>
					{:else}
						<div class="space-y-4">
							{#each filteredProducts as product (product.id)}
								<ProductCard
									mode="list"
									id={product.id}
									title={product.title}
									image={product.images[0]}
									category={product.category}
									price={product.price}
									brand={product.brand}
								/>
							{/each}
						</div>
					{/if}
				</div>
			{:else}
				<div class="flex-grow">
					{#if isGridView}
						<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
							{#each Array(6) as _}
								<Skeleton class="h-64 w-64" />
							{/each}
						</div>
					{:else}
						<div class="space-y-4">
							{#each Array(6) as _}
								<Skeleton class="h-20 w-full" />
							{/each}
						</div>
					{/if}
				</div>
			{/if}
		</div>
	</div>
</div>
