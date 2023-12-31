<script lang="ts">
	import type { GuestGroup } from '$lib/utils/interfaces';
	import {
		type PaginationSettings,
		Paginator,
		modalStore,
		type ModalComponent,
		type ModalSettings,
		ProgressRadial
	} from '@skeletonlabs/skeleton';
	import Group from './Group.svelte';
	import { fetchPost } from '$lib/utils/api';
	import { handleToast } from '$lib/utils/toast';
	import { onMount } from 'svelte';
	import { groups } from '../../../stores/guestsGroupReserved';
	import { _ } from 'svelte-i18n';

	export let groupPerPage: number = 5;

	let loading = true;

	onMount(async () => {
		const res = await fetchPost('group/all');
		$groups = res.groups;
		page.size = $groups.length;
		loading = false;
	});

	let page: PaginationSettings = {
		offset: 0,
		limit: groupPerPage,
		size: $groups.length,
		amounts: []
	};
	$: page.size = $groups.length;

	const onClick = (group: GuestGroup) => {
		const modalComponent: ModalComponent = {
			ref: Group,
			props: { group },
			slot: '<p>Skeleton</p>'
		};
		const modal: ModalSettings = {
			type: 'component',
			component: modalComponent
		};
		modalStore.trigger(modal);
	};

	const addGroup = async (groupName: string) => {
		if (!groupName || groupName === '') return;
		const { ok, data } = await fetchPost('group/add', { groupName });
		handleToast(
			ok,
			$_('pages.reserved-area.group.added'),
			$_('pages.reserved-area.group.notAdded')
		);
		if (ok && data) {
			$groups.push(data[0] as GuestGroup);
			page.size = $groups.length;
		}
	};

	const addGroupModal = () => {
		const modal: ModalSettings = {
			type: 'prompt',
			title: $_('pages.reserved-area.group.add'),
			body: $_('pages.reserved-area.group.addQuestion'),
			value: '',
			valueAttr: { type: 'text', minlength: 3, maxlength: 50, required: true },
			response: async (r: string) => await addGroup(r),
			buttonTextCancel: $_('general.back'),
			buttonTextSubmit: $_('general.confirm'),
			modalClasses: '!bg-white border border-black rounded-xl'
		};
		modalStore.trigger(modal);
	};
</script>

<div class="relative min-h-[480px]">
	{#if loading}
		<div class="flex items-center justify-center min-h-[480px]">
			<ProgressRadial width={'w-12 md:w-20'} />
		</div>
	{:else}
		<div class="flex flex-col space-y-4">
			{#each $groups.slice(page.offset * page.limit, (page.offset + 1) * page.limit) as group}
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<div
					on:click={() => {
						onClick(group);
					}}
					class="p-5 pt-3 border border-black
                rounded-xl hover:bg-slate-100"
				>
					<p class="">{group.name}</p>
				</div>
			{/each}
		</div>
		<div class="absolute bottom-0 right-0">
			<Paginator
				bind:settings={page}
				showPreviousNextButtons={true}
				separatorText={$_('general.of')}
			/>
		</div>
		<div class="absolute bottom-0 left-0">
			<button class="btn btn-sm variant-filled" on:click={addGroupModal}
				>{$_('pages.reserved-area.group.add')}</button
			>
		</div>
	{/if}
</div>
