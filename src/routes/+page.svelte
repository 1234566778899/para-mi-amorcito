<script lang="ts">
	import { tick } from 'svelte';
	import Lightbox, { type MediaItem } from '$lib/components/Lightbox.svelte';
	import Petals from '$lib/components/Petals.svelte';
	import Polaroid from '$lib/components/Polaroid.svelte';
	import Sunflower from '$lib/components/Sunflower.svelte';

	const recuerdos: (MediaItem & { rotate: number; delay: number })[] = [
		{
			src: '/media/foto-1.jpeg',
			alt: 'Nosotros dos en el cine, con la máscara de Spider-Man',
			rotate: -7,
			delay: 0
		},
		{
			src: '/media/video-1.mp4',
			kind: 'video',
			alt: 'Un video nuestro',
			rotate: 2,
			delay: 140
		},
		{
			src: '/media/foto-2.jpeg',
			alt: 'Noche de mascarillas negras, riéndonos',
			rotate: 8,
			delay: 280
		}
	];

	let abierto: number | null = $state(null);
	let ultimoBoton: HTMLElement | null = null;

	function abrir(i: number, boton: HTMLElement) {
		ultimoBoton = boton;
		abierto = i;
	}

	async function cerrar() {
		abierto = null;
		// esperamos a que la tarjeta deje de ser inerte para devolver el foco
		await tick();
		ultimoBoton?.focus();
	}
</script>

<svelte:head>
	<title>Flores amarillas · 21 de septiembre</title>
	<meta
		name="description"
		content="Un ramo de flores amarillas para el 21 de septiembre, hecho de pixeles y de recuerdos."
	/>
</svelte:head>

<Petals />

<main class="tabletop" inert={abierto !== null}>
	<article class="card">
		<!-- flores de fondo, muy tenues -->
		<svg class="pattern" aria-hidden="true">
			<defs>
				<g id="bloom">
					<ellipse rx="9" ry="15" cy="-13" />
					<ellipse rx="9" ry="15" cy="-13" transform="rotate(72)" />
					<ellipse rx="9" ry="15" cy="-13" transform="rotate(144)" />
					<ellipse rx="9" ry="15" cy="-13" transform="rotate(216)" />
					<ellipse rx="9" ry="15" cy="-13" transform="rotate(288)" />
				</g>
				<pattern
					id="blooms"
					width="160"
					height="160"
					patternUnits="userSpaceOnUse"
					patternTransform="rotate(14)"
				>
					<g fill="#fff8d6" opacity="0.55">
						<use href="#bloom" x="34" y="30" />
						<use href="#bloom" x="120" y="74" transform="scale(0.78)" />
						<use href="#bloom" x="72" y="126" transform="scale(1.15)" />
					</g>
				</pattern>
			</defs>
			<rect width="100%" height="100%" fill="url(#blooms)" />
		</svg>

		<!-- girasoles decorativos -->
		<Sunflower class="deco deco--tr" size="min(46vw, 270px)" spin={8} />
		<Sunflower class="deco deco--bl" size="min(66vw, 360px)" spin={22} />
		<Sunflower class="deco deco--bc" size="min(36vw, 205px)" spin={-14} petals={11} />
		<Sunflower class="deco deco--br" size="min(46vw, 260px)" spin={35} petals={13} />

		<div class="content">
			<section class="letter">
				<p class="eyebrow">21 de septiembre</p>
				<h1 class="title">Te amo</h1>

				<p>Cada día a tu lado es un regalo que llena mi vida de amor, alegría y calma…</p>

				<p>
					Deseo que la vida nos siga encontrando juntos, compartiendo felicidad, sueños y momentos
					que nos hagan sentir que lo mejor siempre está por venir.
				</p>

				<p>
					Hoy te regalo estas flores amarillas para recordarte que contigo hasta los días grises se
					pintan de sol, y que quiero seguir celebrando nuestro amor una y mil veces más, tomados de
					la mano y rodeados de un millón de bendiciones.
				</p>
			</section>

			<p class="badge">eres increíble</p>

			<div class="fan">
				{#each recuerdos as recuerdo, i (recuerdo.src)}
					<Polaroid
						src={recuerdo.src}
						kind={recuerdo.kind}
						alt={recuerdo.alt}
						rotate={recuerdo.rotate}
						delay={recuerdo.delay}
						onopen={(boton) => abrir(i, boton)}
					/>
				{/each}
			</div>

		</div>
	</article>
</main>

{#if abierto !== null}
	<Lightbox
		items={recuerdos}
		index={abierto}
		onclose={cerrar}
		onnavigate={(i) => (abierto = i)}
	/>
{/if}

<style>
	/* La tarjeta ocupa exactamente la pantalla: nada de scroll. */
	.tabletop {
		height: 100dvh;
		overflow: hidden;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.card {
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
		background: linear-gradient(170deg, #fbe47f 0%, #f7d95f 55%, #f6d251 100%);
		animation: rise 900ms cubic-bezier(0.22, 1, 0.36, 1) both;
	}

	.pattern {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		opacity: 0.42;
		pointer-events: none;
	}

	.card :global(.deco) {
		position: absolute;
		z-index: 1;
	}

	.card :global(.deco--tr) {
		top: 0;
		right: 0;
		transform: translate(30%, -24%);
	}

	.card :global(.deco--bl) {
		bottom: 0;
		left: 0;
		transform: translate(-30%, 8%);
	}

	.card :global(.deco--bc) {
		bottom: 0;
		left: 28%;
		transform: translate(0, 40%);
	}

	.card :global(.deco--br) {
		bottom: 0;
		right: 0;
		transform: translate(24%, 10%);
	}

	.content {
		position: relative;
		z-index: 2;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		padding: clamp(0.9rem, 3.2dvh, 2.5rem) clamp(1rem, 5vw, 2rem);
	}

	/* ---------- la carta ---------- */
	.eyebrow {
		margin: 0 0 0.2rem;
		font-size: clamp(0.58rem, 1.15dvh, 0.72rem);
		font-weight: 500;
		letter-spacing: 0.22em;
		text-transform: uppercase;
		color: #9a7028;
	}

	.title {
		margin: 0 0 clamp(0.4rem, 1.2dvh, 1rem);
		font-family: 'Kaushan Script', cursive;
		font-weight: 400;
		font-size: clamp(2.1rem, 8dvh, 4rem);
		line-height: 0.95;
		color: #4a3116;
		text-shadow: 0 2px 0 rgb(255 255 255 / 0.25);
	}

	.letter p {
		margin: 0 0 clamp(0.5rem, 1.8dvh, 1.1rem);
		font-weight: 300;
		font-size: clamp(0.78rem, 1.9dvh, 1.05rem);
		line-height: 1.55;
		text-align: justify;
		text-wrap: pretty;
		hyphens: auto;
		color: #3b2a18;
	}

	.letter p:last-child {
		margin-bottom: 0;
	}

	/* ---------- el sticker ---------- */
	.badge {
		position: relative;
		z-index: 3;
		align-self: flex-end;
		margin: 0 clamp(0.25rem, 3vw, 2rem) clamp(-1.6rem, -2.2dvh, -0.8rem) 0;
		max-width: 8ch;
		transform: rotate(-8deg);
		font-family: 'Pacifico', cursive;
		font-size: clamp(1.45rem, 5.2dvh, 2.6rem);
		line-height: 1.05;
		text-align: center;
		color: #e7a33f;
		-webkit-text-stroke: 3px #fff6dd;
		paint-order: stroke fill;
		text-shadow: 3px 4px 0 rgb(154 98 24 / 0.55);
	}

	/* ---------- el abanico de fotos ---------- */
	.fan {
		display: flex;
		justify-content: center;
		align-items: flex-start;
	}

	.fan :global(.polaroid) {
		width: clamp(26%, 17dvh, 34%);
	}

	.fan :global(.polaroid + .polaroid) {
		margin-left: -5%;
	}

	.fan :global(.polaroid:nth-child(2)) {
		z-index: 2;
		width: clamp(29%, 18.5dvh, 37%);
		margin-top: 2%;
	}

	@keyframes rise {
		from {
			opacity: 0;
			transform: translateY(24px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	/* ---------- pantallas grandes: la postal sobre la mesa ---------- */
	@media (min-width: 760px) {
		.tabletop {
			padding: clamp(1rem, 3dvh, 2.5rem);
		}

		.card {
			width: auto;
			height: min(100%, 880px);
			aspect-ratio: 5 / 7;
			border-radius: 8px;
			box-shadow:
				0 1px 2px rgb(88 62 20 / 0.18),
				0 18px 40px -12px rgb(88 62 20 / 0.35);
		}

		.content {
			display: grid;
			grid-template-columns: 1.2fr 0.8fr;
			grid-template-rows: 1fr auto;
			align-items: center;
			column-gap: clamp(1rem, 2.5vw, 2rem);
			padding: clamp(1.5rem, 3.5vw, 3rem);
		}

		.letter {
			grid-column: 1;
			grid-row: 1;
			align-self: center;
		}

		.title {
			font-size: clamp(3rem, 6dvh, 4.5rem);
		}

		.letter p {
			font-size: clamp(0.85rem, 1.5dvh, 1.02rem);
			line-height: 1.7;
		}

		.badge {
			grid-column: 1;
			grid-row: 2;
			align-self: end;
			margin: 0 clamp(1rem, 4vw, 3rem) 0 auto;
			font-size: clamp(1.9rem, 4.2dvh, 2.8rem);
		}

		/* en horizontal la tira vuelve a ser vertical, como la plantilla */
		.fan {
			grid-column: 2;
			grid-row: 1 / -1;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			height: 100%;
		}

		.fan :global(.polaroid) {
			width: min(100%, 215px);
		}

		.fan :global(.polaroid + .polaroid) {
			margin-left: 0;
			margin-top: -4%;
		}

		.fan :global(.polaroid:nth-child(2)) {
			width: min(100%, 215px);
			margin-top: -4%;
			margin-left: 9%;
		}

		.fan :global(.polaroid:nth-child(3)) {
			margin-right: 6%;
		}
	}

	/* ---------- pantallas muy bajitas (horizontal): dejamos volver el scroll ---------- */
	@media (max-height: 520px) and (max-width: 900px) {
		.tabletop {
			height: auto;
			min-height: 100dvh;
			overflow: visible;
		}

		.card {
			height: auto;
		}

		.content {
			padding-block: 2rem 3rem;
			gap: 1.5rem;
		}

		.title {
			font-size: 2.4rem;
		}

		.letter p {
			font-size: 0.85rem;
		}

		.badge {
			font-size: 1.6rem;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.card {
			animation: none;
		}
	}
</style>
