<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/bienvenue.webp';
	import welcome_fallback from '$lib/images/bienvenue.png';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<source srcset={welcome} type="image/png" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		sur mon site de <br />test Svelte
	</h1>

	<h2>
		ça c'est la page <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		padding: 0 0 1rem 0;
	}

	.welcome img {
		width: 100%;
		aspect-ratio: 16 / 9;
		display: block;
		z-index: -1;
	}
</style>
