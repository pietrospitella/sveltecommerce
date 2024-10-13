<script lang="ts">
	import { page } from '$app/stores'
	import { Button } from '$lib/components/ui/button'
	import Input from '$lib/components/ui/input/input.svelte'
	import { Skeleton } from '$lib/components/ui/skeleton'
	import { Heart, ShoppingCart, Truck, Minus, Plus } from 'lucide-svelte'
	import { getContext, onMount } from 'svelte'

	$: product = {
		id: 0,
		title: '',
		category: '',
		description: '',
		brand: '',
		price: 0,
		rating: 0,
		reviews: [],
		images: [],
		shippingInformation: ''
	}

	let isLoaded = false
	let count = 1
	let bigImage = ''
	let cartFn: any = getContext('addToCart')

	function addToCart() {
		cartFn(product, count)
		console.log('Added to cart')
	}
	function changeBigImg(idx: number) {
		bigImage = product.images[idx]
	}

	onMount(async () => {
		const res = await fetch(`https://dummyjson.com/products/${$page.params.id}`).then((res) =>
			res.json()
		)

		if (res) {
			console.log(res)
			isLoaded = true
			product = res
			bigImage = product.images[0]
		}
	})
</script>

<svelte:head>
	<title>{product.title} | SvelteCommerce</title>
	<style>
		/* Chrome, Safari, Edge, Opera */
		input::-webkit-outer-spin-button,
		input::-webkit-inner-spin-button {
			-webkit-appearance: none;
			margin: 0;
		}

		/* Firefox */
		input[type='number'] {
			-moz-appearance: textfield;
		}
	</style>
</svelte:head>

<div class="container mx-auto px-4 py-8">
	<div class="grid gap-8 md:grid-cols-2">
		<div>
			{#if isLoaded}
				<img
					src={bigImage}
					alt={product.brand}
					class="mb-4 h-96 w-full rounded-lg object-contain"
				/>
				<div class="grid grid-cols-4 gap-4">
					{#each product.images as img, index}
						<button on:click={() => changeBigImg(index)}>
							<img
								src={img}
								alt="Thumbnail {index}"
								class="h-20 w-full rounded-lg object-contain p-2 {img === bigImage ? 'border' : ''}"
							/>
						</button>
					{/each}
				</div>
			{:else}
				<!-- else content here -->
				<Skeleton class="mb-4 h-96 w-full rounded-lg object-contain" />
				<div class="grid grid-cols-4 gap-4">
					<Skeleton class="h-20 w-full rounded-lg object-contain p-2" />
					<Skeleton class="h-20 w-full rounded-lg object-contain p-2" />
					<Skeleton class="h-20 w-full rounded-lg object-contain p-2" />
				</div>
			{/if}
		</div>
		<div>
			{#if isLoaded}
				<div class="mb-4 flex items-center">
					<img
						src="https://cdn.brandfetch.io/{product.brand}.com/w/400/h/400"
						alt="{product.brand} logo"
						class="mr-2 h-8 w-8 rounded-full"
					/>
					<span class="font-semibold">{product.brand}</span>
				</div>
				<h1 class="mb-2 text-3xl font-bold">{product.title}</h1>
				<p class="mb-4 text-sm text-gray-500">{product.description}</p>
				<div class="mb-4 flex items-center">
					<div class="flex">
						{#each Array(Math.ceil(product.rating)) as _}
							<svg class="h-5 w-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
								<path
									d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
								/>
							</svg>
						{/each}
						{#each Array(5 - Math.ceil(product.rating)) as _}
							<svg class="h-5 w-5 text-gray-300" fill="currentColor" viewBox="0 0 20 20">
								<path
									d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
								/>
							</svg>
						{/each}
					</div>
					<span class="ml-2 text-sm text-gray-500">
						{product.reviews.length}
						{product.reviews.length === 1 ? 'review' : 'reviews'}
					</span>
				</div>
				<p class="mb-6 text-3xl font-bold">${product.price}</p>
				<div class="mb-4 flex w-full">
					<Button
						variant="default"
						disabled={count < 2}
						on:click={() => {
							if (count >= 2) {
								count -= 1
							}
						}}
					>
						<Minus class="h-4 w-4" />
					</Button>
					<Input
						type="number"
						readonly
						min={1}
						max={5}
						bind:value={count}
						class="mx-4 w-12 text-center"
					/>
					<Button
						variant="default"
						disabled={count > 4}
						on:click={() => {
							if (count <= 4) {
								count += 1
							}
						}}
					>
						<Plus class="h-4 w-4" />
					</Button>
				</div>
				<Button class="mb-4 w-full" on:click={addToCart}>
					<ShoppingCart class="mr-2 h-4 w-4" />
					Add to cart
				</Button>
				<Button variant="outline" class="mb-4 w-full">
					<Heart class="mr-2 h-4 w-4" />
					Add to favorites
				</Button>
				<div class="flex items-center text-sm text-gray-500">
					<Truck class="mr-2 h-4 w-4" />
					{product.shippingInformation}
				</div>
			{:else}
				<div class="mb-4 flex items-center">
					<Skeleton class="mr-2 h-8 w-8 rounded-full"/>
					<Skeleton class="w-20 h-4"/>
				</div>
				<Skeleton class="mb-2 w-40 h-8"/>
				<Skeleton class="mb-4 w-96 h-16"/>
				<div class="mb-4 flex items-center">
					<Skeleton class="ml-2 w-20 h-4"/>
				</div>
				<Skeleton class="mb-6 w-24 h-12"/>
				<div class="mb-4 flex w-full">
					<Button
						variant="default"
						disabled={count < 2}
						on:click={() => {
							if (count >= 2) {
								count -= 1
							}
						}}
					>
						<Minus class="h-4 w-4" />
					</Button>
					<Input
						type="number"
						readonly
						min={1}
						max={5}
						bind:value={count}
						class="mx-4 w-12 text-center"
					/>
					<Button
						variant="default"
						disabled={count > 4}
						on:click={() => {
							if (count <= 4) {
								count += 1
							}
						}}
					>
						<Plus class="h-4 w-4" />
					</Button>
				</div>
				<Button class="mb-4 w-full" on:click={addToCart}>
					<ShoppingCart class="mr-2 h-4 w-4" />
					Add to cart
				</Button>
				<Button variant="outline" class="mb-4 w-full">
					<Heart class="mr-2 h-4 w-4" />
					Add to favorites
				</Button>
				<div class="flex items-center text-sm text-gray-500">
					<Truck class="mr-2 h-4 w-4" />
					{product.shippingInformation}
				</div>
			{/if}
		</div>
	</div>
</div>
