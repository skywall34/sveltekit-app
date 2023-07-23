<!-- svelte-ignore a11y-click-events-have-key-events -->

<script>
	import { goto } from '$app/navigation';

	function gotoAbout() {
		goto('/about');
	}

	function goToChat() {
		goto('/chat');
	}

	function goToBlog() {
		goto('/blog');
	}
</script>

<div class="container">
	<div id="whoami" class="section" on:click={gotoAbout}>
		<div class="content">
			<h1>Who am I</h1>
		</div>
		<div class="overlay" />
	</div>
	<div id="blog" class="section" on:click={goToBlog}>
		<div class="content">
			<h1>Blog</h1>
		</div>
		<div class="overlay" />
	</div>
	<div id="contact" class="section" on:click={goToChat}>
		<div class="content">
			<h1>Chat</h1>
		</div>
		<div class="overlay" />
	</div>
</div>

<style>
	* {
		box-sizing: border-box;
	}

	/* body {
		margin: 0;
		padding: 0;
		font-family: 'Montserrat', sans-serif;
	} */

	.container {
		display: flex;
		height: 100vh;
	}

	.section {
		flex: 1;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100%;
		overflow: hidden;
		background-size: cover;
		background-position: center;
		color: #fff;
		transition: flex 0.4s ease;
		position: relative;
	}

	.section .overlay {
		background-color: rgba(0, 0, 0, 0.7);
		width: 100%;
		height: 100%;
		position: absolute;
		transition: background-color 0.8s ease;
	}

	.section .content {
		z-index: 2;
	}

	.section:hover {
		flex: 2;
	}

	.section:hover .overlay {
		background-color: rgba(0, 0, 0, 0.95);
	}

	#whoami {
		background-image: url(https://images.unsplash.com/photo-1522205987242-8e22924ab42a?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=c0f679eb8f15705d46ea90008f39642b&auto=format&fit=crop&w=500&q=60);
	}

	#blog {
		background-image: url(https://images.unsplash.com/photo-1530893609608-32a9af3aa95c?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=fe6b92f88f55824e64a1bae15f5bf52a&auto=format&fit=crop&w=500&q=60);
	}

	#contact {
		background-image: url(https://images.unsplash.com/photo-1517799094725-e3453440724e?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=353f3663a9fae75773d2942aeb37c2c8&auto=format&fit=crop&w=500&q=60);
	}
</style>
