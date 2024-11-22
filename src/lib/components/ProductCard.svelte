<script lang="ts">
	import { goto } from '$app/navigation'

  interface Props {
    mode?: 'grid' | 'list';
    id?: number;
    category?: string;
    image?: string;
    price?: number;
    title?: string;
    brand?: string;
  }

  let {
    mode = 'grid',
    id = 0,
    category = 'Device',
    image = '/placeholder.svg?height=300&width=300',
    price = 34.0,
    title = 'Product Title',
    brand = ''
  }: Props = $props();
</script>

<button onclick={() => goto(`/products/${id}`)} type="button" class="{mode === 'list' ? 'w-full' : ''}">
  {#if mode === "grid"}
    <div
      class="w-full overflow-hidden rounded-xl bg-white transition-transform duration-300 hover:scale-105 sm:w-64 md:w-72 lg:w-80"
    >
      <img src={image} alt={title} class="h-48 w-full object-contain sm:h-56 md:h-64" />
      <div class="p-4">
        <p class="text-lg font-bold sm:text-xl">$ {price.toFixed(2)}</p>
        <h3 class="line-clamp-2 h-10 text-sm font-medium sm:text-base">{title}</h3>
        <p class="text-xs text-gray-500 sm:text-sm">{brand}</p>
      </div>
    </div>
  {:else}
     <div class="rounded-lg p-4 flex items-center transition-transform duration-300 hover:scale-105">
      <img src={image} alt={title} class="h-16 w-16 object-contain" />
      <div class="text-left ml-4">
        <h3 class="text-lg font-semibold">{title}</h3>
        <p class="text-sm text-gray-600">{category}</p>
        <p class="text-sm text-gray-600">{brand}</p>
      </div>
      <p class="text-lg font-bold ml-auto">${price.toFixed(2)}</p>
      </div>
  {/if}
</button>