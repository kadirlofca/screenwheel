<script lang="ts">
	import { tick } from 'svelte';
	import type { CollectionEntry } from "astro:content";

	export let items: CollectionEntry<'sections'>[];
	let selectedItem: CollectionEntry<'sections'> = items[0];
	let scrollContainer: HTMLDivElement;

	function getIndexDistanceToSelectedItem(index: number): number {
		const selectedIndex = items.findIndex(i => i.id === selectedItem.id);
		return Math.abs(selectedIndex - index);
	}

	function mapValueExponential(
		value: number,
		inputMin: number,
		inputMax: number,
		outputMin: number,
		outputMax: number,
		exponent = 2
	): number {
		const t = (value - inputMin) / (inputMax - inputMin);
		const curved = 1 - Math.pow(1 - t, exponent);
		return outputMin + (outputMax - outputMin) * curved;
	}

	async function centerElementInView(el: HTMLElement) {
		await tick();

		const container = scrollContainer;
		const containerHeight = container.clientHeight;
		const elOffsetTop = el.offsetTop;
		const elHeight = el.offsetHeight;

		const targetScrollTop = elOffsetTop - (containerHeight / 2) + (elHeight / 2);
		const duration = 500;

		let start: number | null = null;
		const initialScroll = container.scrollTop;
		const distance = targetScrollTop - initialScroll;

		function animateScroll(timestamp: number) {
			if (!start) start = timestamp;
			const elapsed = timestamp - start;
			const progress = Math.min(elapsed / duration, 1);
			const eased = 1 - Math.pow(1 - progress, 3);

			container.scrollTop = initialScroll + distance * eased;

			if (progress < 1) {
				requestAnimationFrame(animateScroll);
			}
		}

		requestAnimationFrame(animateScroll);
	}
</script>

<style>
	.scrollbar-hide::-webkit-scrollbar {
		display: none;
	}
	.scrollbar-hide {
		scrollbar-width: none;
		-ms-overflow-style: none;
	}
</style>

<div class="flex flex-col justify-between p-8 text-right text-primary-foreground bg-primary">
	<h1 class="text-xl">Name Lastname</h1>

	<div
		bind:this={scrollContainer}
		class="relative h-full overflow-y-scroll scrollbar-hide">

    	<div class="h-1/2 shrink-0 pointer-events-none"></div>

		<div class="flex flex-col items-end gap-6">
			{#each items as item, index}
				<button
					on:click={(e) => {
						selectedItem = item;
						centerElementInView(e.currentTarget);
					}}
					class="h-[40px] text-5xl transition-all duration-500 ease-in-out"
					style={
						"opacity: " +
						mapValueExponential(getIndexDistanceToSelectedItem(index), 0, items.length, 1, (-0.05 * items.length)) +
						// "; transform: rotateX(" +
						// mapValueExponential(getIndexDistanceToSelectedItem(index), 0, items.length, 0, 80, 3) + "deg) " +
                        "; font-size: " + 
                        mapValueExponential(getIndexDistanceToSelectedItem(index), 0, items.length, 3, 2) + "rem"
					}
				>
					{item.data.title}
				</button>
			{/each}
		</div>

    	<div class="h-1/2 shrink-0 pointer-events-none"></div>

	</div>

    <footer>
        <a class="text-xl" href="mailto:user@email.com">user@email.com</a>
        <p class="text-sm opacity-60">Site design & development by Kadir Lofca.</p>
    </footer>
</div>
