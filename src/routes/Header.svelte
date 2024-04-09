<script>
	import { page } from '$app/stores';
	import logo from '$lib/images/allenyade-logo.svg';
	import github from '$lib/images/github.svg';
</script>

<header>
	<div class="corner">
		<a href="https://danyella.works">
			<img src={logo} alt="Allenyade" />
		</a>
	</div>

	<nav>
		<ul>
			<li aria-current={$page.url.pathname === '/' ? 'page' : undefined}>
				<a href="/">Home</a>
			</li>
			<li aria-current={$page.url.pathname === '/about' ? 'page' : undefined}>
				<a href="/about">About</a>
			</li>
			<li aria-current={$page.url.pathname.startsWith('/sverdle') ? 'page' : undefined}>
				<a href="/sverdle">Sverdle</a>
			</li>
		</ul>
	</nav>

	<div class="corner">
		<a href="https://github.com/sveltejs/kit">
			<img src={github} alt="GitHub" />
		</a>
	</div>
</header>

<style>
	header {
		display: flex;
		justify-content: space-between;
	}

	.corner {
		width: 3em;
		height: 3em;
	}

	.corner a {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}

	.corner img {
		width: 2em;
		height: 2em;
		object-fit: contain;
	}

	nav {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		display: flex;
		justify-content: center;
		--background: #fff3;
		z-index: 99;
		pointer-events: none;
	}

	ul {
		position: relative;
		padding: 1rem;
		margin: 2rem;
		height: 3em;
		display: flex;
		justify-content: space-around;
		align-items: center;
		list-style: none;
		background: var(--background);
		background-size: contain;
		border: 0.22rem solid #fff2;
		box-shadow: #fff6 0 0 64px 20px;
		border-radius: 100vw;
		width: 66.67%;
		backdrop-filter: blur(15px);
		z-index: 99;
		pointer-events: auto;
	}

	li {
		position: relative;
		height: 100%;
	}

	li[aria-current='page']::before {
		--_size: 6px;
		content: '';
		width: 50%;
		height: var(--_size);
		position: absolute;
		bottom: calc(var(--_size) / 2);
		left: 50%;
		translate: -50% 0;
		border-radius: 100vw;
		background-color: var(--color-theme-1);
	}

	nav a {
		display: flex;
		height: 100%;
		align-items: center;
		padding: 0 0.5rem;
		color: var(--color-text);
		font-weight: 700;
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		text-decoration: none;
		transition: color 0.33s linear;
	}

	a:hover {
		color: var(--color-theme-1);
	}
</style>
