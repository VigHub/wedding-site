<script lang="ts">
	import { page } from '$app/stores';
	import { _ } from 'svelte-i18n';
	import { base } from '$app/paths';
	import { drawerStore } from '@skeletonlabs/skeleton';

	function drawerClose(): void {
		drawerStore.close();
	}
	// const activeClass = 'bg-primary-400';
	const activeClass = 'underline underline-offset-4 decoration-1';

	$: items = [
		{
			text: $_('navbar.welcome'),
			url: `${base}/`,
			color: $page.url.pathname === `${base}/` ? activeClass : ''
		},
		{
			text: $_('navbar.gift-list'),
			url: `${base}/gift-list`,
			color: $page.url.pathname === `${base}/gift-list` ? activeClass : ''
		},
		{
			text: $_('navbar.confirm-presence'),
			url: `${base}/confirm-presence`,
			color: $page.url.pathname === `${base}/confirm-presence` ? activeClass : ''
		},
		{
			text: $_('navbar.info'),
			url: `${base}/info`,
			color: $page.url.pathname === `${base}/info` ? activeClass : ''
		},
		{
			text: $_('navbar.reserved-area'),
			url: `${base}/reserved-area`,
			color: [`${base}/reserved-area`, `${base}/guest-messages`].includes($page.url.pathname)
				? activeClass
				: ''
		}
	];
</script>

<div class="relative flex p-6">
	<h1 class="">Menù</h1>
	<button
		class="absolute right-5"
		on:click={() => {
			drawerStore.close();
		}}
	>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			fill="none"
			viewBox="0 0 24 24"
			stroke-width="1.5"
			stroke="currentColor"
			class="w-6 h-6"
		>
			<path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
		</svg>
	</button>
</div>
<hr />
<nav class="ps-6 uppercase">
	<ul>
		{#each items as item (item.url)}
			<li class={`pt-8 rounded-xl  ${item.color}`}>
				<a class="text-lg" href={item.url} on:click={drawerClose}>{item.text}</a>
			</li>
		{/each}
	</ul>
</nav>
