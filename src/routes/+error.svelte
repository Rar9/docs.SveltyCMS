<script lang="ts">
	// Stores
	import { page } from '$app/stores';
	// Components
	import SveltyCMSLogo from '@lib/SveltyCMS_Logo.svelte';

	// ParaglideJS
	import * as m from '@src/paraglide/messages';

	const speed = 50;
	const size = 140;
	const font = 0.9;
	const repeat = 3;
	const separator = ' • ';

	let array: string[] = [];
	$: array = Array.from({ length: repeat }, () => 'SveltyCMS' + separator)
		.join('')
		.split('');
</script>

{#if $page}
	<main class="flex h-screen w-full flex-col items-center justify-center bg-gradient-to-t from-surface-900 via-surface-700 to-surface-900 text-white">
		<div class="relative">
			<!-- Rotating SiteName -->
			<div class="seal absolute" style="--size: {size}px; --speed: {speed * 1000}ms; --font: {font}em">
				{#each array as char, index}
					<!-- Angle calculation -->
					<div class="char" style="--angle: {`${(1 / array.length) * index}turn`}">
						<span class={index >= array.length - 6 && index <= array.length - 4 ? 'text-primary' : ''}>
							{char}
						</span>
					</div>
				{/each}
			</div>
			<!-- Site Logo -->
			<SveltyCMSLogo fill="red" className="absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 h-16 mb-2" />
		</div>

		<div class="relative">
			<!-- Error class -->
			<h1 class="relative text-9xl font-extrabold tracking-widest text-white">
				{$page.status}
			</h1>
			<!-- Error url  -->
			<div
				class="absolute left-1/2 top-1/2 mx-auto -translate-x-1/2 -translate-y-1/2 rotate-12 transform rounded-md bg-error-600/80 px-2 text-center text-sm font-bold text-white"
			>
				<div class="min-w-[200px]">{$page.url}</div>
				<div class="whitespace-nowrap">{m.error_pagenotfound()}</div>
			</div>
		</div>

		<h1 class="text-center text-4xl font-extrabold tracking-widest text-surface-400">
			{#if $page.error}
				{$page.error.message}
			{/if}
		</h1>

		<p class="text-lg text-white">{m.error_wrong()}</p>
		<a
			href="/"
			data-sveltekit-preload-data="tap"
			class="relative mt-5 block rounded-full bg-gradient-to-br from-error-700 via-error-600 to-error-700 px-8 py-4 font-bold uppercase text-white shadow-xl"
		>
			{m.error_gofrontpage()}
		</a>
	</main>
{/if}

<style lang="postcss">
	@keyframes rotation {
		0% {
			transform: rotate(0turn);
		}
		100% {
			transform: rotate(1turn);
		}
	}

	.seal {
		position: relative;
		width: var(--size);
		height: var(--size);
		border-radius: 100%;
		animation: rotation var(--speed) linear infinite;
		font-size: var(--font);
	}
	.char {
		width: 1em;
		height: 100%;
		position: absolute;
		top: 0;
		left: 50%;
		transform: translateX(-50%) rotate(var(--angle, 0deg));
		text-align: center;
		text-transform: uppercase;
	}
</style>
