<script lang="ts">
	export interface MediaItem {
		src: string;
		kind?: 'image' | 'video';
		alt?: string;
	}

	interface Props {
		items: MediaItem[];
		index: number;
		onclose: () => void;
		onnavigate: (index: number) => void;
	}

	let { items, index, onclose, onnavigate }: Props = $props();

	const item = $derived(items[index]);

	let closeButton: HTMLButtonElement | null = $state(null);
	let video: HTMLVideoElement | null = $state(null);
	let touchStartX = 0;

	$effect(() => {
		closeButton?.focus();
	});

	// El video se abre con sonido: para eso se tocó.
	$effect(() => {
		if (video) video.play().catch(() => {});
	});

	function go(step: number) {
		onnavigate((index + step + items.length) % items.length);
	}

	function onkeydown(event: KeyboardEvent) {
		if (event.key === 'Escape') onclose();
		else if (event.key === 'ArrowRight') go(1);
		else if (event.key === 'ArrowLeft') go(-1);
	}

	function ontouchstart(event: TouchEvent) {
		touchStartX = event.changedTouches[0].clientX;
	}

	function ontouchend(event: TouchEvent) {
		const dx = event.changedTouches[0].clientX - touchStartX;
		if (Math.abs(dx) > 50) go(dx < 0 ? 1 : -1);
	}
</script>

<svelte:window {onkeydown} />

<div
	class="overlay"
	role="dialog"
	aria-modal="true"
	aria-label="Recuerdo en pantalla completa"
	tabindex="-1"
	{ontouchstart}
	{ontouchend}
>
	<button class="backdrop" onclick={onclose} aria-label="Cerrar"></button>

	<button class="round close" bind:this={closeButton} onclick={onclose} aria-label="Cerrar">
		<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6 6l12 12M18 6L6 18" /></svg>
	</button>

	{#if items.length > 1}
		<button class="round nav prev" onclick={() => go(-1)} aria-label="Anterior">
			<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M14.5 5.5L8 12l6.5 6.5" /></svg>
		</button>
		<button class="round nav next" onclick={() => go(1)} aria-label="Siguiente">
			<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M9.5 5.5L16 12l-6.5 6.5" /></svg>
		</button>
	{/if}

	<div class="stage">
		{#key index}
			{#if item.kind === 'video'}
				<!-- svelte-ignore a11y_media_has_caption -->
				<video
					bind:this={video}
					src={item.src}
					autoplay
					loop
					playsinline
					controls
					aria-label={item.alt}
				></video>
			{:else}
				<img src={item.src} alt={item.alt} />
			{/if}
		{/key}
	</div>

	{#if item.alt}
		<p class="caption">{item.alt}</p>
	{/if}
</div>

<style>
	.overlay {
		position: fixed;
		inset: 0;
		z-index: 50;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 0.75rem;
		padding: clamp(0.75rem, 4vw, 2rem);
		background: rgb(38 24 6 / 0.9);
		backdrop-filter: blur(6px);
		animation: fade 200ms ease both;
	}

	.backdrop {
		position: absolute;
		inset: 0;
		border: none;
		padding: 0;
		background: none;
		cursor: zoom-out;
	}

	.stage {
		position: relative;
		display: flex;
		max-width: 100%;
		max-height: calc(100dvh - 6.5rem);
	}

	.stage :global(img),
	.stage :global(video) {
		max-width: min(100%, 900px);
		max-height: calc(100dvh - 6.5rem);
		width: auto;
		height: auto;
		object-fit: contain;
		border-radius: 4px;
		background: #1c1305;
		box-shadow: 0 24px 60px rgb(0 0 0 / 0.55);
		animation: pop 280ms cubic-bezier(0.2, 0.9, 0.3, 1.15) both;
	}

	.caption {
		position: relative;
		margin: 0;
		max-width: 34ch;
		text-align: center;
		font-size: 0.8rem;
		font-weight: 300;
		line-height: 1.4;
		color: #f7e7c0;
		text-shadow: 0 1px 3px rgb(0 0 0 / 0.5);
		pointer-events: none;
	}

	.round {
		position: absolute;
		z-index: 2;
		display: grid;
		place-items: center;
		width: 2.75rem;
		height: 2.75rem;
		border: none;
		border-radius: 50%;
		color: #432c10;
		background: rgb(255 255 255 / 0.9);
		box-shadow: 0 4px 14px rgb(0 0 0 / 0.35);
		cursor: pointer;
		-webkit-tap-highlight-color: transparent;
	}

	.round svg {
		width: 1.2rem;
		height: 1.2rem;
		fill: none;
		stroke: currentColor;
		stroke-width: 2;
		stroke-linecap: round;
		stroke-linejoin: round;
	}

	.round:active {
		transform: scale(0.93);
	}

	.close {
		top: clamp(0.75rem, 3vw, 1.5rem);
		right: clamp(0.75rem, 3vw, 1.5rem);
	}

	.nav {
		top: 50%;
		margin-top: -1.375rem;
	}

	.prev {
		left: clamp(0.5rem, 2.5vw, 1.5rem);
	}

	.next {
		right: clamp(0.5rem, 2.5vw, 1.5rem);
	}

	@keyframes fade {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}

	@keyframes pop {
		from {
			opacity: 0;
			transform: scale(0.94);
		}
		to {
			opacity: 1;
			transform: scale(1);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.overlay,
		.stage :global(img),
		.stage :global(video) {
			animation: none;
		}
	}
</style>
