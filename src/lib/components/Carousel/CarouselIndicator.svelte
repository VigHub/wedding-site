<script lang="ts">
	import carouselIndex from '../../../stores/carousel';

	export let index: number;
	export let handleInterval: () => void;

	const onClick = () => {
		handleInterval();
		carouselIndex.set(index);
	};
</script>

{#if index === $carouselIndex}
	<button
		type="button"
		class="mx-[3px] box-content h-[3px] w-[30px] flex-initial
                cursor-pointer border-0 border-y-[10px] border-solid border-transparent
                 bg-white bg-clip-padding p-0 -indent-[999px] transition-opacity
                 duration-[600ms] ease-[cubic-bezier(0.25,0.1,0.25,1.0)] motion-reduce:transition-none"
		aria-label={`Slide ${index}`}
		on:click={onClick}
	/>
{:else}
	<button
		type="button"
		class="mx-[3px] box-content h-[3px] w-[30px] flex-initial cursor-pointer
        border-0 border-y-[10px] border-solid border-transparent
        bg-white bg-clip-padding p-0 -indent-[999px] opacity-50
         transition-opacity duration-[600ms] ease-[cubic-bezier(0.25,0.1,0.25,1.0)]
         motion-reduce:transition-none"
		aria-label={`Slide ${index}`}
		on:click={onClick}
	/>
{/if}
