<script lang="ts">
	import { goto } from '$app/navigation';
	import { base } from '$app/paths';
	import canAccessReservedArea from '../../stores/reserved';
	import { fetchPost } from '$lib/utils/api';
	import { _ } from 'svelte-i18n';
	import { sendToastError } from '$lib/utils/toast';

	let password = '';

	function sha512(str: string) {
		return crypto.subtle.digest('SHA-512', new TextEncoder().encode(str)).then((buf) => {
			return Array.prototype.map
				.call(new Uint8Array(buf), (x) => ('00' + x.toString(16)).slice(-2))
				.join('');
		});
	}

	const onClick = async () => {
		if (password === '') return;
		const hashed = await sha512(password);
		const { authenticated } = await fetchPost('auth', { hashed });
		if (authenticated) {
			canAccessReservedArea.set(true);
			goto(`${base}/guest-messages`);
		} else {
			sendToastError($_('pages.reserved-area.wrongPwd'));
			password = '';
		}
	};
</script>

<div class="text-center text-4xl">
	<h1 class="">
		{$_('pages.reserved-area.title')} <span class="invisible md:visible">🔐</span>
	</h1>
	<h1 class="visible md:invisible">🔐</h1>
</div>
<div class="container h-[300px] mx-auto my-auto flex justify-center items-center align-middle">
	<form class="w-full md:w-1/4 space-y-2">
		<label for="password" class="text-base"
			>{$_('pages.reserved-area.msg1')} <br /> {$_('pages.reserved-area.msg2')} 🤵🏻‍♂️👰🏻‍♀️</label
		>
		<input
			id="password"
			type="password"
			class="w-full p-2 block rounded-lg focus:ring-blue-500 focus:border-blue-500"
			bind:value={password}
		/>
		<button
			type="submit"
			class="btn variant-filled align-baseline float-right ms-4"
			on:click={onClick}>{$_('general.send')}</button
		>
	</form>
</div>
