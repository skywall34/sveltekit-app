<script>
	import { goto } from '$app/navigation';
	import { currentUser } from '../lib/pocketbase';
	import { signOut } from './login/+page.svelte';
    import Toggle from './toggle.svelte'
    import * as config from '$lib/config'

	function handleLogout() {
		signOut();
		goto('/login');
	}
</script>

<nav>
    <!-- Title -->
	<a href="/" class="title">
		<b>{config.title}</b>
	</a>

    <ul class="links">
        <a href="/">Home</a>
        {#if $currentUser}
            <a href="/" on:click|preventDefault={handleLogout}>Logout</a>
        {:else}
            <a href="/login">Login</a>
        {/if}
    </ul>

    <!-- Theme -->
    <Toggle />
</nav>

<style>
	nav {
		padding-block: var(--size-7);
	}

	.links {
		margin-block: var(--size-7);
	}

	a {
		color: inherit;
		text-decoration: none;
	}

	@media (min-width: 768px) {
		nav {
			display: flex;
			justify-content: space-between;
		}

		.links {
			display: flex;
			gap: var(--size-7);
			margin-block: 0;
		}
	}
</style>
