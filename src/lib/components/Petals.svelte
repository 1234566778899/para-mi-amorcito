<script lang="ts">
	// Pétalos que caen suavemente sobre toda la página.
	const petals = Array.from({ length: 11 }, (_, i) => ({
		left: (i * 7.3 + 3) % 100,
		size: 7 + ((i * 5) % 9),
		duration: 11 + ((i * 3) % 9),
		delay: -(i * 1.7),
		drift: i % 2 === 0 ? 40 : -55,
		spin: i % 3 === 0 ? 360 : -300
	}));
</script>

<div class="petals" aria-hidden="true">
	{#each petals as p, i (i)}
		<span
			class="petal"
			style="left:{p.left}%; width:{p.size}px; height:{p.size * 1.7}px;
			       animation-duration:{p.duration}s; animation-delay:{p.delay}s;
			       --drift:{p.drift}px; --spin:{p.spin}deg"
		></span>
	{/each}
</div>

<style>
	.petals {
		position: fixed;
		inset: 0;
		overflow: hidden;
		pointer-events: none;
		z-index: 5;
	}

	.petal {
		position: absolute;
		top: -12vh;
		border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
		background: linear-gradient(160deg, #ffe9a3, #f3c33c);
		opacity: 0.5;
		animation-name: fall;
		animation-timing-function: linear;
		animation-iteration-count: infinite;
	}

	@keyframes fall {
		from {
			transform: translate3d(0, 0, 0) rotate(0deg);
		}
		to {
			transform: translate3d(var(--drift), 115vh, 0) rotate(var(--spin));
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.petals {
			display: none;
		}
	}
</style>
