<script lang="ts">
	import { getAttendance } from '$lib/utils/guests';
	import type { GuestMessageWithGuest } from '$lib/utils/interfaces';
	import { format } from 'date-fns';
	import { _ } from 'svelte-i18n';

	const getStringTime = (created: Date) => {
		return format(new Date(created), 'dd/MM/yyyy HH:mm');
	};
	export let guestMessage: GuestMessageWithGuest;
	export let inModal = false;
	const messageClass = inModal
		? 'max-h-52 min-h-[200px] overflow-auto my-3 max-w-[800px] md:px-3 '
		: 'max-h-12 overflow-hidden';
</script>

<div
	class="flex flex-col p-5 pt-3 border border-black
				 rounded-xl hover:bg-slate-100 bg-white"
>
	<p class="text-sm text-gray-500 ms-auto mt-auto right-0 top-0 pb-1">
		{$_(getAttendance(guestMessage.attendance))}
	</p>
	<p class={messageClass}>{guestMessage.message}</p>
	<div class="flex">
		<p class="truncate hover:text-clip w-1/2 md:w-3/5 text-sm text-gray-500 me-auto left-0">
			{guestMessage.guest.name}
			{guestMessage.guest.surname}
		</p>
		<p class="text-sm text-gray-500 ms-auto right-0">
			{getStringTime(guestMessage.created)}
		</p>
	</div>
</div>
