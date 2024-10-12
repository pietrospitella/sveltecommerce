<script>
	import '../app.css';
	import { Popover, PopoverContent, PopoverTrigger } from '$lib/components/ui/popover'
    import Search from "lucide-svelte/icons/search"
    import ShoppingCart from "lucide-svelte/icons/shopping-cart"
	import { Input } from '$lib/components/ui/input'
	import { page } from '$app/stores'

	console.log($page);
</script>

<div class="min-h-screen bg-background text-foreground">
	<header class="bg-primary p-4 text-primary-foreground fixed top-0 left-0 right-0 z-20">
        <nav class="container mx-auto flex items-center justify-between">
            <a href="/" class="text-2xl font-bold">
				<img src="/src/lib/images/logo.png" alt="" class="h-8"/>
			</a>
            <div class="relative mx-auto self-center flex-1 grow-0 sm:block hidden">
                <Search class="text-muted-foreground absolute left-2.5 top-2.5 h-4 w-4" />
                <Input
                  type="search"
                  placeholder="Search..."
                  class="bg-amber-100 focus:bg-background w-[0px] rounded-lg pl-8 md:w-[200px] lg:w-[320px]"
                />
            </div>
			<Popover>
				<PopoverTrigger>
					<ShoppingCart/>
				</PopoverTrigger>
				<PopoverContent sideOffset={10}>
					<span>nothing here.</span>
				</PopoverContent>
			</Popover>
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