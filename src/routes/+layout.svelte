<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

<style>
	:global(*, *::before, *::after) {
		box-sizing: border-box;
	}

	:global(html) {
		-webkit-text-size-adjust: 100%;
	}

	/* la tarjeta ocupa la pantalla entera: sin scroll */
	:global(html, body) {
		overflow: hidden;
		overscroll-behavior: none;
	}

	@media (max-height: 520px) and (max-width: 900px) {
		:global(html, body) {
			overflow: auto;
		}
	}

	:global(body) {
		margin: 0;
		height: 100dvh;
		font-family: 'Poppins', system-ui, -apple-system, 'Segoe UI', sans-serif;
		color: #3b2a18;
		background: #efece5;
		background-image:
			radial-gradient(circle at 18% 12%, #fbf9f5 0%, transparent 45%),
			radial-gradient(circle at 82% 88%, #e6e1d6 0%, transparent 55%);
		background-attachment: fixed;
	}

	:global(img, video, svg) {
		max-width: 100%;
	}
</style>
