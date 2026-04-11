<script>
	import '../app.css';
	import '@fontsource/inter';
	import '@fontsource/inter/300.css';
	import '@fontsource/inter/400.css';
	import '@fontsource/inter/500.css';
	import '@fontsource/inter/600.css';
	import '@fontsource/inter/700.css';
	import '@fontsource/inter/800.css';

	import Social from '$lib/Social.svelte';

	import { dev } from '$app/environment';
	import { inject } from '@vercel/analytics';
	inject({ mode: dev ? 'development' : 'production' });

	import { browser } from '$app/environment';

	let darkMode = false;

	function handleSwitchDarkMode() {
		darkMode = !darkMode;

		localStorage.setItem('theme', darkMode ? 'dark' : 'light');

		darkMode
			? document.documentElement.classList.add('dark')
			: document.documentElement.classList.remove('dark');
	}

	if (browser) {
		if (
			localStorage.theme === 'dark' ||
			(!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)
		) {
			document.documentElement.classList.add('dark');
			darkMode = true;
		} else {
			document.documentElement.classList.remove('dark');
			darkMode = false;
		}
	}
</script>

<div class="relative flex flex-col min-h-screen justify-center bg-grad dark:bg-grad-dark dark:text-white">
	<button
		on:click={handleSwitchDarkMode}
		class="fixed top-5 right-5 opacity-40 hover:opacity-70 transition-opacity"
	>
		{#if darkMode}
			<i class="far fa-sun" />
		{:else}
			<i class="far fa-moon" />
		{/if}
	</button>
	<div
		class="flex flex-col w-4/5 md:max-w-2xl justify-center m-auto py-16 leading-relaxed space-y-4"
	>
		<slot />
	</div>
	<Social />
	<div class="flex justify-center pb-6">
		<p class="text-xs opacity-50">made with lots of ☕️</p>
	</div>
</div>
