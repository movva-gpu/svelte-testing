<script lang="ts">
	import { spring } from 'svelte/motion';

	let count = 0;
	let clicked = false;

	const displayed_count = spring();
	$: displayed_count.set(count);
	$: offset = modulo($displayed_count, 1);

	function modulo(n: number, m: number) {
		// handle negative numbers
		return ((n % m) + m) % m;
	}

	const counting = (e: MouseEvent, decreasing = false, isAlt = false, isMaj = false) => {
		if (!clicked) clicked = true;

		if (e.altKey) {
			isAlt = true;
		}
		if (e.shiftKey) {
			isMaj = true;
		}

		if (isAlt && isMaj) count += 100 * (!decreasing ? 1 : -1);
		else if (isAlt) count += 0.1 * (!decreasing ? 1 : -1);
		else if (isMaj) count += 10 * (!decreasing ? 1 : -1);
		else count += 1 * (!decreasing ? 1 : -1);

		if (!isAlt) count = Math.floor(count);
		count = Math.min(999, Math.max(-999, count));
	};
</script>

<div class="why {Math.abs(count) === 999 ? 'shown' : 'hidden'}">
	<h3>Why?</h3>
</div>

<div class="counter">
	<button on:click={(e) => counting(e, true)} aria-label="Decrease the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5" />
		</svg>
	</button>

	<div class="counter-viewport">
		<div class="counter-digits" style="transform: translate(0, {100 * offset}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor($displayed_count + 1)}</strong>
			<strong>{Math.floor($displayed_count)}</strong>
		</div>
	</div>

	<button on:click={(e) => counting(e)} aria-label="Increase the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
		</svg>
	</button>
</div>

<div class="fold {clicked ? 'shown' : 'hidden'}">
	<div class="reset">
		<button class="reset-btn" on:click={() => (count = 0)}><span>Reset</span></button>
	</div>

	<div class="tooltip" aria-hidden="true">
		<p>
			You can press
			<kbd>
				<button
					on:click|preventDefault={(e) => counting(e, false, false, true)}
					on:contextmenu|preventDefault={(e) => counting(e, true, false, true)}
					><span>MAJ</span></button
				></kbd
			>,
			<kbd>
				<button
					on:click|preventDefault={(e) => counting(e, false, true, false)}
					on:contextmenu|preventDefault={(e) => counting(e, true, true, false)}
					><span>ALT</span>
				</button>
			</kbd>
			or
			<kbd>
				<button
					on:click|preventDefault={(e) => counting(e, false, true, true)}
					on:contextmenu|preventDefault={(e) => counting(e, true, true, true)}
					><span>ALT+MAJ</span>
				</button>
			</kbd>
			to control how much you count!
		</p>
	</div>
</div>

<!-- Styles -->
<style>
	.counter {
		display: flex;
		border-top: 1px solid rgba(0, 0, 0, 0.1);
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		margin: 1rem 0;
	}

	.counter button {
		width: 2em;
		aspect-ratio: 1;
		padding: 0;
		margin: 0.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 0;
		background-color: transparent;
		touch-action: manipulation;
		font-size: 2rem;
		transition: all 0.67s;

		&:hover {
			rotate: 180deg;
		}
	}

	svg {
		width: 33%;
		height: 33%;
	}

	path {
		vector-effect: non-scaling-stroke;
		stroke-width: 2px;
		stroke: #444;
	}

	button:not(.counter *) {
		--_hover-size: 0.25rem;
		--_hover-rotation: 20deg;
		cursor: pointer;
		color: var(--color-theme-1);
		padding: 0.8rem 3rem;
		border: 1px solid var(--color-theme-1);
		border-radius: 0.4rem;
		background: none;
		box-shadow: inset var(--color-theme-1) 0 0 0;
		transition-property: box-shadow, background-color;
		transition-duration: 0.33s;
		transition-timing-function: ease-in;

		&:hover {
			background-color: #f0f0f0;
			box-shadow: inset var(--color-theme-1) 0 calc(-1 * var(--_hover-size)) 0;
			transition-timing-function: ease-out;
		}

		& span {
			display: inline-block;
			transition-property: translate, transform;
			transition-duration: 0.33s;
			transition-timing-function: ease-in;
		}

		&:hover span {
			translate: 0 calc(-1 * var(--_hover-size));
			transform: rotateX(var(--_hover-rotation));
			transition-timing-function: ease-out;
		}
	}

	.counter-viewport {
		width: 8em;
		height: 6em;
		overflow: hidden;
		text-align: center;
		position: relative;
	}

	.counter-viewport strong {
		position: absolute;
		display: flex;
		width: 100%;
		height: 100%;
		font-weight: 400;
		color: var(--color-theme-1);
		font-size: 4rem;
		align-items: center;
		justify-content: center;
	}

	.counter-digits {
		position: absolute;
		width: 100%;
		height: 100%;
	}

	.hidden {
		top: -100%;
		user-select: none;
	}

	.fold {
		backface-visibility: hidden;
		transition: transform 1s ease;
		transform-origin: top;

		& .tooltip kbd button {
			font-weight: bolder;
			padding: 0.33rem;
			--_hover-size: 0.15rem;
			--_hover-rotation: 10deg;
		}

		& .reset {
			width: fit-content;
			margin: auto;
		}

		&.shown {
			transform: rotateX(0deg);
		}

		&.hidden {
			transform: rotateX(-95deg);
		}
	}

	.why {
		&.hidden {
			height: 0;
			transform: rotateY(90deg);

			transition:
				transform 0.33s ease,
				height 0.33s 0.17s ease;
		}

		&.shown {
			height: 64px;
			transform: rotateY(0deg);

			transition:
				transform 0.33s 0.17s ease,
				height 0.33s ease;
		}
	}
</style>
