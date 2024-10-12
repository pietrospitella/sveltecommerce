<script lang="ts">
	import { goto } from "$app/navigation"

    export let id: number = 0;
    export let imageUrl: string = "/placeholder.svg?height=300&width=300";
    export let price: number = 34.00;
    export let name: string = "Stripe Details Jersey Track Top";
    export let availabilityStatus: string = "Men's shoes";
  </script>
  
  <button on:click={() => goto(`/products/${id}`)} class="w-full sm:w-64 md:w-72 lg:w-80 bg-white rounded-xl overflow-hidden transition-transform duration-300 hover:scale-105" type="button">
    <img src={imageUrl} alt={name} class="w-full h-48 sm:h-56 md:h-64 object-contain" />
    <div class="p-4">
      <p class="text-lg sm:text-xl font-bold">$ {price.toFixed(2)}</p>
      <h3 class="text-sm sm:text-base font-medium line-clamp-2 h-10">{name}</h3>
      <p class="text-xs sm:text-sm text-gray-500">{availabilityStatus}</p>
    </div>
  </button>
