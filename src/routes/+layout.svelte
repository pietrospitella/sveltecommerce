<script lang="ts">
	import '../app.css';
	import { Popover, PopoverContent, PopoverTrigger } from '$lib/components/ui/popover'
    import Search from "lucide-svelte/icons/search"
    import ShoppingCart from "lucide-svelte/icons/shopping-cart"
	import { Input } from '$lib/components/ui/input'
	import { page } from '$app/stores'
	import { setContext } from 'svelte'
	import Badge from '$lib/components/ui/badge/badge.svelte'
	import Button from '$lib/components/ui/button/button.svelte'
	import { Tooltip, TooltipContent, TooltipTrigger } from '$lib/components/ui/tooltip'
	import { goto } from '$app/navigation'
	import { Toaster } from "$lib/components/ui/sonner"
	import { toast } from "svelte-sonner"	

	interface Props {
		children?: import('svelte').Snippet;
	}

	let { children }: Props = $props();

	interface IProduct {
		id: number
		title: string
		category: string
		description: string
		brand: string
		price: number | bigint | any
		rating: number
		reviews: any[]
		images: string[],
		shippingInformation: string,
		count: number
	}

	let cart: IProduct[] = $state([])

	function addToCart (product: IProduct, count: number) {
		// Check if the product already exists in the cart
		const existingProductIndex = cart.findIndex(item => item.id === product.id);
		
		if (existingProductIndex !== -1) {
			// If it exists, check if adding the count exceeds 5
			if (cart[existingProductIndex].count + count <= 5) {
				cart[existingProductIndex].count += count;
			} else {
				toast.error("Cannot buy more than 5 of the same product")
			}
		} else {
			// If it doesn't exist, add it to the cart
			let cartProduct = {...product, count: count};
			cart = [...cart, cartProduct];
		}
	}

	setContext('addToCart', addToCart)
	// console.log($page);
</script>

<Toaster richColors closeButton position="top-right"/>
<div class="min-h-screen bg-background text-foreground">
	<header class="bg-primary p-4 text-primary-foreground fixed top-0 left-0 right-0 z-20">
        <nav class="container mx-auto flex items-center justify-between">
            <a href="/" class="text-2xl font-bold">
				<img src="/src/lib/images/logo.svg" alt="" class="h-8"/>
			</a>
            <div class="relative mx-auto self-center flex-1 grow-0 sm:block hidden">
                <Search class="text-muted-foreground absolute left-2.5 top-2.5 h-4 w-4" />
                <Input
                  type="search"
                  placeholder="Search..."
                  class="bg-amber-100 focus:bg-background w-[0px] rounded-lg pl-8 md:w-[200px] lg:w-[320px]"
                />
            </div>
			<Tooltip>
				<TooltipTrigger>
					<Popover>
				<PopoverTrigger class="relative">
					<ShoppingCart/>
					{#if cart.length > 0}
						 <Badge variant="destructive" class="absolute -top-2 -right-2 text-xs w-4 h-4 p-1">{cart.reduce((total, item) => total + item.count, 0)}</Badge>
					{/if}
				</PopoverTrigger>
				<PopoverContent sideOffset={10} class={cart.length > 0 ? 'w-auto' : ''}>
					<h3 class="text-lg font-bold mb-4">Cart</h3>
					{#if cart.length > 0}
						{#each cart as cartItem, index}
						<div class="flex flex-1 items-center rounded-lg border p-4 {index !== cart.length - 1 ? 'mb-4' : ''}">
							<img class="w-10 h-10 object-contain" src={cartItem.images[0]} alt={cartItem.title}>
							<div class="flex flex-1 flex-col">
								<div class="flex flex-1 justify-between">
									<h3 class="text-sm font-semibold">{cartItem.title}</h3>
									<p class="text-md font-bold ml-4">${(cartItem.price * cartItem.count).toFixed(2)}</p>
								</div>
								<div class="flex flex-1 justify-between">
									<p class="text-xs text-gray-600">{cartItem.category} (x{cartItem.count})</p>
									<span>
										<Button class="size-1 p-2" onclick={() => addToCart(cartItem, -1)} disabled={cartItem.count <= 1}>-</Button>
										<span class="mx-2">{cartItem.count}</span>
										<Button class="size-1 p-2" onclick={() => addToCart(cartItem, 1)} disabled={cartItem.count >= 5}>+</Button>
									</span>
								</div>
							</div>
						</div>
						{/each}
						<div class="flex gap-4 mt-4">
							<Button><ShoppingCart class="mr-4"/>Checkout</Button>
							<Button variant="secondary">Continue Shopping</Button>
						</div>
						{:else}
						<div class="flex flex-col">
							<span class="mb-2">Nothing here.</span>
							<Button onclick={() => goto('/cart')}><ShoppingCart class="mr-4"/>Go to Cart</Button>
						</div>
					{/if}
				</PopoverContent>
			</Popover>
				</TooltipTrigger>
				<TooltipContent>Cart</TooltipContent>
			</Tooltip>
			
		</nav>
	</header>
	<div class="mx-auto mt-[72px] min-h-screen">
		{@render children?.()}
	</div>
	<footer class="mt-12 bg-primary p-4 text-primary-foreground">
		<div class="container mx-auto text-center">
			<p>&copy; 2024 SvelteCommerce. All rights reserved.</p>
		</div>
	</footer>
</div>