<script lang="ts">
	import { Button } from '$lib/components/ui/button'
	import { Card } from '$lib/components/ui/card'
	import { Input } from '$lib/components/ui/input'

	interface CartItem {
		id: number
		name: string
		price: number
		quantity: number
	}

	let cartItems = $state<CartItem[]>([])

	let totalPrice = $derived(
		cartItems.reduce((total: number, item: CartItem) => total + item.price * item.quantity, 0)
	)

	function updateQuantity(id: number, newQuantity: number) {
		if (newQuantity >= 0) {
			cartItems = cartItems.map((item) =>
				item.id === id ? { ...item, quantity: newQuantity } : item
			)
		}
	}

	function removeItem(id: number) {
		cartItems = cartItems.filter((item) => item.id !== id)
	}
</script>

<div class="container mx-auto p-4">
	<h1 class="mb-4 text-2xl font-bold">Your Cart</h1>
	{#if cartItems.length === 0}
		<p>Your cart is empty.</p>
	{:else}
		{#each cartItems as item (item.id)}
			<Card class="mb-4 p-4">
				<div class="flex items-center justify-between">
					<div>
						<h2 class="text-lg font-semibold">{item.name}</h2>
						<p class="text-gray-600">${item.price.toFixed(2)}</p>
					</div>
					<div class="flex items-center">
						<Button
							variant="outline"
							size="sm"
							onclick={() => updateQuantity(item.id, item.quantity - 1)}>-</Button
						>
						<Input
							type="number"
							min="0"
							class="mx-2 w-16 text-center"
							value={item.quantity}
							oninput={(e: any) => updateQuantity(item.id, parseInt(e.currentTarget.value) || 0)}
						/>
						<Button
							variant="outline"
							size="sm"
							onclick={() => updateQuantity(item.id, item.quantity + 1)}>+</Button
						>
						<Button
							variant="destructive"
							size="sm"
							class="ml-4"
							onclick={() => removeItem(item.id)}>Remove</Button
						>
					</div>
				</div>
			</Card>
		{/each}
		<div class="mt-4 text-right">
			<p class="text-xl font-bold">Total: ${totalPrice.toFixed(2)}</p>
			<Button class="mt-2">Proceed to Checkout</Button>
		</div>
	{/if}
</div>
