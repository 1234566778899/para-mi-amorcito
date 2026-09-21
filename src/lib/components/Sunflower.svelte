<script lang="ts">
	interface Props {
		/** Tamaño del girasol (cualquier unidad CSS) */
		size?: string;
		/** Rotación inicial de la corola */
		spin?: number;
		/** Cantidad de pétalos de la corona exterior */
		petals?: number;
		class?: string;
		style?: string;
	}

	let { size = '160px', spin = 0, petals = 12, class: cls = '', style = '' }: Props = $props();

	const uid = $props.id();
	const outer = $derived(Array.from({ length: petals }, (_, i) => (360 / petals) * i + spin));
	const inner = $derived(
		Array.from({ length: petals }, (_, i) => (360 / petals) * i + spin + 360 / petals / 2)
	);
</script>

<svg
	class="sunflower {cls}"
	style="width:{size};height:{size};{style}"
	viewBox="0 0 200 200"
	aria-hidden="true"
	focusable="false"
>
	<defs>
		<radialGradient id="petal-{uid}" cx="50%" cy="15%">
			<stop offset="0%" stop-color="#fff0b8" />
			<stop offset="100%" stop-color="#f3c23c" />
		</radialGradient>
		<radialGradient id="petal-in-{uid}" cx="50%" cy="15%">
			<stop offset="0%" stop-color="#ffe89c" />
			<stop offset="100%" stop-color="#eeb32b" />
		</radialGradient>
		<radialGradient id="heart-{uid}" cx="38%" cy="34%">
			<stop offset="0%" stop-color="#7a5226" />
			<stop offset="65%" stop-color="#5b3a1c" />
			<stop offset="100%" stop-color="#3d2510" />
		</radialGradient>
		<pattern id="seeds-{uid}" width="8" height="8" patternUnits="userSpaceOnUse">
			<circle cx="2" cy="2" r="1.3" fill="#2b1806" />
			<circle cx="6" cy="6" r="1.3" fill="#8a6031" />
		</pattern>
	</defs>

	<g transform="translate(100 100)">
		{#each outer as angle (angle)}
			<ellipse rx="16" ry="47" cy="-51" fill="url(#petal-{uid})" transform="rotate({angle})" />
		{/each}
		{#each inner as angle (angle)}
			<ellipse rx="13" ry="34" cy="-40" fill="url(#petal-in-{uid})" transform="rotate({angle})" />
		{/each}
		<circle r="31" fill="url(#heart-{uid})" />
		<circle r="31" fill="url(#seeds-{uid})" opacity="0.3" />
		<circle r="31" fill="none" stroke="#d99c2b" stroke-width="2.5" opacity="0.55" />
	</g>
</svg>

<style>
	.sunflower {
		display: block;
		pointer-events: none;
		filter: drop-shadow(0 4px 10px rgb(146 104 12 / 0.16));
	}
</style>
