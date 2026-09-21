<script lang="ts">
	interface Props {
		src: string;
		kind?: 'image' | 'video';
		alt?: string;
		/** Inclinación de la foto dentro del abanico */
		rotate?: number;
		/** Retraso de la animación de entrada */
		delay?: number;
		/** Se toca la foto: abrirla a pantalla completa */
		onopen?: (trigger: HTMLElement) => void;
	}

	let { src, kind = 'image', alt = '', rotate = 0, delay = 0, onopen }: Props = $props();

	let video: HTMLVideoElement | null = $state(null);

	// Safari/Chrome a veces no arrancan el autoplay al primer intento.
	$effect(() => {
		if (video) video.play().catch(() => {});
	});
</script>

<button
	class="polaroid"
	style="--rot:{rotate}deg; --delay:{delay}ms"
	onclick={(event) => onopen?.(event.currentTarget)}
	aria-label={kind === 'video' ? `Ver el video: ${alt}` : `Ver la foto: ${alt}`}
>
	<span class="frame">
		{#if kind === 'video'}
			<video bind:this={video} {src} autoplay loop muted playsinline preload="metadata"></video>
			<span class="play" aria-hidden="true">
				<svg viewBox="0 0 24 24"><path d="M9 7.5l8 4.5-8 4.5z" /></svg>
			</span>
		{:else}
			<img {src} alt="" loading="eager" decoding="async" />
		{/if}
	</span>
</button>

<style>
	.polaroid {
		display: block;
		margin: 0;
		padding: 0;
		border: none;
		background: none;
		cursor: pointer;
		animation: drop 700ms cubic-bezier(0.2, 0.9, 0.3, 1.2) both;
		animation-delay: var(--delay);
		transition: filter 180ms ease;
		-webkit-tap-highlight-color: transparent;
	}

	.polaroid:hover,
	.polaroid:focus-visible {
		filter: brightness(1.04);
	}

	.polaroid:focus-visible {
		outline: 3px solid #fff6dd;
		outline-offset: 3px;
	}

	.polaroid:active .frame {
		transform: scale(0.97);
	}

	.frame {
		position: relative;
		display: block;
		background: #fff;
		padding: 5.5%;
		border-radius: 2px;
		box-shadow:
			0 2px 4px rgb(92 62 16 / 0.15),
			0 10px 22px rgb(92 62 16 / 0.22);
		transition: transform 180ms ease;
	}

	img,
	video {
		display: block;
		width: 100%;
		aspect-ratio: 4 / 5;
		object-fit: cover;
		background: #e9dcc0;
	}

	.play {
		position: absolute;
		right: 11%;
		bottom: 11%;
		width: clamp(1.5rem, 3.2dvh, 2rem);
		height: clamp(1.5rem, 3.2dvh, 2rem);
		display: grid;
		place-items: center;
		border-radius: 50%;
		color: #5b3a1c;
		background: rgb(255 255 255 / 0.85);
		box-shadow: 0 2px 6px rgb(0 0 0 / 0.25);
	}

	.play svg {
		width: 62%;
		height: 62%;
		fill: currentColor;
	}

	@keyframes drop {
		from {
			opacity: 0;
			transform: translateY(-18px) rotate(calc(var(--rot) * 2.2)) scale(0.92);
		}
		to {
			opacity: 1;
			transform: translateY(0) rotate(var(--rot)) scale(1);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.polaroid {
			animation: none;
			transform: rotate(var(--rot));
		}
	}
</style>
