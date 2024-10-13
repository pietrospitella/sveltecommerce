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

	let cart: IProduct[] = []

	function addToCart (product: IProduct, count: number) {
		let cartProduct = {...product, count: count}
		console.log(cartProduct)
		
		cart = [...cart, cartProduct]
	}

	setContext('addToCart', addToCart)
	// console.log($page);
</script>

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
						 <Badge variant="destructive" class="absolute -top-2 -right-2 text-xs w-4 h-4 p-1">{cart.length}</Badge>
					{/if}
				</PopoverTrigger>
				<PopoverContent sideOffset={10} class="{cart.length > 0 ? 'w-auto' : ''}">
					<h3 class="text-lg font-bold mb-4">Cart</h3>
					{#if cart.length > 0}
						{#each cart as cartItem, index}
						<div class="flex items-center rounded-lg border p-4 {index !== cart.length - 1 ? 'mb-4' : ''}">
							<img class="w-10 h-10 object-contain" src={cartItem.images[0]} alt={cartItem.title}>
							<div>
								<h3 class="text-sm font-semibold">{cartItem.title}</h3>
								<p class="text-xs text-gray-600">{cartItem.category} (x{cartItem.count})</p>
							</div>
							<p class="text-md font-bold ml-auto">${cartItem.price.toFixed(2) * cartItem.count}</p>
						</div>	1
						{/each}
						<div class="flex gap-4 mt-4">
							<Button><ShoppingCart class="mr-4"/>Checkout</Button>
							<Button variant="secondary">Continue Shopping</Button>
						</div>
					{:else}
						<span>nothing here.</span>
					{/if}
				</PopoverContent>
			</Popover>
				</TooltipTrigger>
				<TooltipContent>Cart</TooltipContent>
			</Tooltip>
			
		</nav>
	</header>
	<div class="mx-auto mt-[72px]">
		<slot></slot>
	</div>
	<footer class="mt-12 bg-primary p-4 text-primary-foreground">
		<div class="container mx-auto text-center">
			<p>&copy; 2024 SvelteCommerce. All rights reserved.</p>
		</div>
	</footer>
</div>