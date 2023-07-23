<script lang="ts">
	import { onMount } from 'svelte';
	import resume from '$lib/assets/resume.pdf';

	let scrollPosition = 0;

	function handleScroll(event: Event) {
		const target = event.target as HTMLElement;
		scrollPosition = target.scrollTop;
	}

	onMount(() => {
		const container = document.getElementById('about-me-container');
		if (container) {
			container.addEventListener('scroll', handleScroll);
		}
	});
</script>

<div class="about-wrapper">
	<div class="about-left">
		<div class="about-left-content">
			<div>
				<div class="shadow">
					<div class="about-img">
						<!-- svelte-ignore a11y-img-redundant-alt -->
						<img src="https://avatars.githubusercontent.com/u/19407051?v=4" alt="github image" />
					</div>
				</div>

				<h2>Mike Shin</h2>
				<h3>Data Engineer</h3>
			</div>

			<ul class="icons">
				<a href="https://www.linkedin.com/in/shindohyun/"><li><i class="fab fa-linkedin" /></li></a>
				<a href="https://www.instagram.com/michael_shin34/"
					><li><i class="fab fa-instagram" /></li></a
				>
			</ul>
		</div>
	</div>

	<div class="about-right">
		<h1>hi<span>!</span></h1>
		<h2>Here's who I am & what I do</h2>
		<div class="about-btns">
			<a href={resume} download class="btn btn-pink">resume</a>
			<a href="https://github.com/skywall34" class="btn btn-white">projects</a>
		</div>

		<div class="about-para">
			<p>
				I graduated from the University of Texas at Austin with a Master's degree in Information
				Technology and Management. My current responsibilities have allowed me to learn plenty of
				skills, from helping build data pipelines to structuring and mentoring new data engineers in
				designing and implementing production ready data platforms. I'm always open to connect with
				companies and individuals that share similar passions in technology both within and outside
				the data engineering field.
			</p>
			<p>
				I also have a passion about learning new technologies, from web development to designing and
				implementing entire data pipelines! So don't feel shy to reach out just to talk if there's
				anything interesting you would like to share!
			</p>
			<p>I'm also happily married, and much too interested in cats, video games, and good food.</p>
		</div>
	</div>
</div>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700;800&display=swap');

	* {
		padding: 0;
		margin: 0;
		box-sizing: border-box;
	}
	.about-wrapper {
		height: 100vh;
	}
	.about-left {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
	}
	.about-left-content > div {
		padding: 4rem 4rem 2.5rem 5rem;
		text-align: center;
	}
	.about-left-content {
		box-shadow: 0px 0px 18px -1px rgba(0, 0, 0, 0.39);
		-webkit-box-shadow: 0px 0px 18px -1px rgba(0, 0, 0, 0.39);
		-moz-box-shadow: 0px 0px 18px -1px rgba(0, 0, 0, 0.39);
	}
	.about-img img {
		display: block;
		width: 200px;
	}
	.about-img {
		width: 200px;
		height: 200px;
		overflow: hidden;
		border-radius: 50%;
		transition: all 0.5s ease-in-out;
	}
	.shadow {
		margin-left: auto;
		margin-right: auto;
		border-radius: 50%;
		width: 200px;
		height: 200px;
		position: relative;
		cursor: pointer;
	}
	.shadow::after {
		content: '';
		position: absolute;
		background: rgba(0, 0, 0, 0.4);
		left: 50%;
		transform: translateX(-50%);
		bottom: -20px;
		height: 7px;
		width: 50px;
		filter: blur(3px);
		-webkit-filter: blur(3px);
		opacity: 0;
		transition: all 0.6s ease;
	}
	.shadow:hover .about-img {
		transform: translateY(-10px);
	}
	.shadow:hover::after {
		opacity: 1;
	}
	.about-left-content h2 {
		font-size: 2rem;
		margin: 2.2rem 0 0.6rem 0;
		line-height: 1.2;
		padding-bottom: 1rem;
		border-bottom: 2px solid #edffec;
	}
	.about-left-content h3 {
		text-transform: uppercase;
		font-weight: 300;
		letter-spacing: 5px;
		margin-top: 1.2rem;
	}
	.icons {
		display: flex;
		justify-content: center;
		padding: 0.8rem 0;
		border-bottom: 2px solid #edffec;
	}
	.icons li {
		list-style-type: none;
		background: #f5524a;
		color: #fff;
		width: 40px;
		height: 40px;
		margin: 0 0.5rem;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 1.2rem;
		cursor: pointer;
		transition: all 0.5s ease-in-out;
	}
	.icons li:hover {
		background: #edffec;
		color: #000;
	}
	.about-right {
		height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 0 5rem;
		text-align: center;
	}
	.about-right h1 {
		font-size: 7rem;
		text-transform: uppercase;
	}
	.about-right h1 span {
		color: #dd2d4a;
	}
	.about-right h2 {
		font-weight: 600;
	}
	.about-btns {
		display: flex;
		margin: 2rem 0;
	}
	.btn {
		border: none;
		font-size: 0.9rem;
		text-transform: uppercase;
		border: 2px solid #000;
		border-radius: 20px;
		padding: 0.55rem 0;
		width: 130px;
		font-weight: 600;
		background: transparent;
		margin: 0 0.5rem;
		cursor: pointer;
	}
	.btn.btn-pink {
		background: #dd2d4a;
		color: #fff;
		border-color: #dd2d4a;
		transition: all 0.5s ease-in-out;
	}
	.btn.btn-pink:hover {
		background: transparent;
		border-color: #dd2d4a;
		color: #000;
	}
	.btn.btn-white {
		transition: all 0.5s ease-in-out;
	}
	.btn.btn-white:hover {
		background: #dd2d4a;
		border-color: #dd2d4a;
		color: #fff;
	}
	.about-para p {
		font-weight: 300;
		padding: 0.5rem;
		opacity: 0.8;
	}

	@media screen and (min-width: 992px) {
		.about-wrapper {
			display: grid;
			grid-template-columns: repeat(2, 1fr);
		}
		.about-left {
			position: relative;
		}
		.about-left-content {
			position: absolute;
			right: -50px;
		}
	}
</style>
