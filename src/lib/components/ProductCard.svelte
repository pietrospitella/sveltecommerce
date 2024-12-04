<script lang="ts">
	import { Heart } from 'lucide-svelte'

	import { Button } from '$lib/components/ui/button'
	import { goto } from '$app/navigation'

	interface Props {
		mode?: 'grid' | 'list'
		id?: number
		category?: string
		image?: string
		price?: number
		title?: string
		brand?: string
		rating?: number
	}

	let {
		mode = 'grid',
		id = 0,
		category = 'Device',
		image = '/placeholder.svg?height=300&width=300',
		price = 34.0,
		title = 'Product Title',
		brand = '',
		rating = 0
	}: Props = $props()
</script>

<button onclick={() => goto(`/products/${id}`)} class={mode === 'list' ? 'w-full' : ''}>
	{#if mode === 'grid'}
		<div
			class="w-full overflow-hidden rounded-xl transition-transform duration-300 sm:w-64 md:w-72 lg:w-80"
		>
			<div class="rounded-xl bg-slate-100 p-8 relative">
        <img src={image} alt={title} class="h-48 w-full object-contain sm:h-56 md:h-64" />
				<Button variant="ghost" class="absolute right-2 top-2 rounded-full z-50" onclick={(e: any) => {e.preventDefault()}}>
					<Heart />
				</Button>
			</div>
			<div class="p-4">
				<div class="flex items-center justify-between">
					<h3 class="line-clamp-2 text-lg font-bold sm:text-xl">{title}</h3>
					<p class="text-lg font-bold sm:text-xl">$ {price.toFixed(2)}</p>
				</div>
				<p class=" mb-1 flex text-xs text-gray-500 sm:text-sm">{brand}</p>
				<div class="flex">
					{#each Array(Math.ceil(rating)) as _}
						<svg class="h-5 w-5 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
							<path
								d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
							/>
						</svg>
					{/each}
					{#each Array(5 - Math.ceil(rating)) as _}
						<svg class="h-5 w-5 text-gray-300" fill="currentColor" viewBox="0 0 20 20">
							<path
								d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
							/>
						</svg>
					{/each}
				</div>
				<!-- <Button class="flex mt-2">Add to Cart</Button> -->
			</div>
		</div>
	{:else}
		<div class="flex items-center rounded-lg p-4 transition-transform duration-300">
			<img src={image} alt={title} class="h-16 w-16 object-contain" />
			<div class="ml-4 text-left">
				<h3 class="text-lg font-semibold">{title}</h3>
				<p class="text-sm text-gray-600">{category}</p>
				<p class="text-sm text-gray-600">{brand}</p>
				<p class="text-sm text-yellow-500">⭐⭐⭐⭐⭐ (121)</p>
				<!-- <Button class="mt-2">Add to Cart</Button> -->
			</div>
			<p class="ml-auto text-lg font-bold">${price.toFixed(2)}</p>
		</div>
	{/if}
</button>
