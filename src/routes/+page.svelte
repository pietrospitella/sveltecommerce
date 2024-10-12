<script lang="ts">
	import { onMount } from 'svelte'
	import { Button } from '$lib/components/ui/button'
	import ProductCard from '$lib/components/ProductCard.svelte'
	import { goto } from '$app/navigation'
	import Carousel from '$lib/components/ui/carousel/carousel.svelte'
	import CarouselContent from '$lib/components/ui/carousel/carousel-content.svelte'
	import CarouselItem from '$lib/components/ui/carousel/carousel-item.svelte'
	import { AspectRatio } from '$lib/components/ui/aspect-ratio'
	import Autoplay from 'embla-carousel-autoplay'
	import CarouselPrevious from '$lib/components/ui/carousel/carousel-previous.svelte'
	import CarouselNext from '$lib/components/ui/carousel/carousel-next.svelte'

    let products: any = []

	const banners = [
		{
			img: '/src/lib/images/hero_1.jpg',
			title: 'Amazing Deals',
			sub: 'Discover our latest tech offerings',
			cta: {
				text: 'Shop Now',
				fn: () => {goto('/products')}
			}
		},
		{
			img: '/src/lib/images/hero_2.jpg',
			title: 'iOS Mania',
			sub: '',
			cta: {
				text: 'See all',
				fn: () => {return}
			}
		},
		{
			img: '/src/lib/images/hero_3.jpg',
			title: 'Fast Repair',
			sub: 'Available on US only',
			cta: {
				text: 'Contact Us',
				fn: () => {return}
			}
		}
	]

	onMount(() => {
        let response
        fetch('https://dummyjson.com/products/category/smartphones?limit=4')
        .then(res => response = res.json())
        .then(data => {
            console.log(data);
            if (data) {
                products = data.products;
            }
        })
	})
</script>

<svelte:head>
	<title>🛒 SvelteCommerce</title>
	<style>
		.banner-gradient::before {
			content: '';
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			background: linear-gradient(to right, rgba(0, 0, 0, 0.7) 0%, rgba(0, 0, 0, 0) 100%);
			z-index: 1;
		}
	</style>
</svelte:head>

<section>
	<Carousel
		opts={{
			loop: true,
			align: 'start'
		}}
		plugins={[Autoplay({
			stopOnMouseEnter: true
		})]}
	>
		<CarouselPrevious class="absolute left-4 z-10"/>
		<CarouselNext class="absolute right-4 z-10"/>
		<CarouselContent class="h-96 ml-0">
			{#each banners as banner, index}
			<CarouselItem class="banner-gradient relative pl-0">
					<!-- <AspectRatio ratio={16 / 9} class="bg-muted"> -->
						<img src={banner.img} alt={`Banner ${banner.title}`} class="h-full w-full object-cover" />
					<!-- </AspectRatio> -->
					<div class="absolute top-0 w-full h-full flex justify-center items-start p-20 flex-col z-10 text-white">
						<h2 class="mb-4 text-4xl font-bold">{banner.title}</h2>
						<p class="mb-6 text-xl">{banner.sub}</p>
						<Button variant="secondary" size="lg" on:click={banner.cta.fn}>{banner.cta.text}</Button>
					</div>
				</CarouselItem>
			{/each}
			
		</CarouselContent>
	</Carousel>
</section>

<main class="container mx-auto px-4 py-8">
	<section>
		<h2 class="mb-6 text-2xl font-semibold">Featured Products</h2>
		<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8 p-4">
			{#each products as product}
			<ProductCard 
				id={product.id}
				name={product.title}
				price={product.price} 
				imageUrl={product.images[0]}
				availabilityStatus={product.brand}
			/>
			{/each}
		</div>
	</section>
</main>

	

