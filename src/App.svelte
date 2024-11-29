<script lang="ts">
	import Map from '$lib/components/Map.svelte';
	import Restaurant from '$lib/components/Restaurant.svelte';
	import Button from '$lib/components/ui/button/button.svelte';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu/index.js';
	import { ArrowRight, ChevronDown } from 'lucide-svelte';
	import Typewriter from 'svelte-typewriter';
	import './app.css';
	import restaurants from './restaurants.json';
	import Input from '$lib/components/ui/input/input.svelte';
	import { onMount } from 'svelte';
	import anime from 'animejs';

	let showPassword = true;
	let showIntro = false;
	let showMap = false;
	let showButton = false;

	let filter = 'all';
	let password = '';

	function mainIntoView(id) {
		const el = document.querySelector(`#${id}`);
		if (!el) return;
		el.scrollIntoView({
			behavior: 'smooth'
		});
	}
	let resFilters = ['all', ...restaurants.reduce((acc, r) => acc.add(r.cuisine), new Set())];

	let canvasEl;
	let ctx;
	let animation;
	const numberOfParticles = 100;
	const particles = [];
	const colors = ['#FFFFFF', '#CCFFFF', '#F0F8FF', '#E0FFFF'];

	function setCanvasSize() {
		canvasEl.width = window.innerWidth * 2;
		canvasEl.height = window.innerHeight * 2;
		canvasEl.style.width = `${window.innerWidth}px`;
		canvasEl.style.height = `${window.innerHeight}px`;
		ctx.scale(2, 2);
	}

	function createParticle() {
		return {
			x: anime.random(0, window.innerWidth),
			y: anime.random(-50, window.innerHeight),
			size: anime.random(2, 5),
			speedY: anime.random(1, 3),
			speedX: anime.random(-1, 1),
			color: colors[anime.random(0, colors.length - 1)],
			draw() {
				ctx.beginPath();
				ctx.arc(this.x, this.y, this.size, 0, 2 * Math.PI);
				ctx.fillStyle = this.color;
				ctx.fill();
			}
		};
	}

	function updateParticles() {
		particles.forEach((p) => {
			p.y += p.speedY;
			p.x += p.speedX;

			// Reset particle when it moves out of canvas
			if (p.y > window.innerHeight) {
				p.y = anime.random(-50, 0);
				p.x = anime.random(0, window.innerWidth);
			}

			p.draw();
		});
	}

	onMount(() => {
		ctx = canvasEl.getContext('2d');
		setCanvasSize();

		// Create initial particles
		for (let i = 0; i < numberOfParticles; i++) {
			particles.push(createParticle());
		}

		// Animation loop
		animation = anime({
			duration: Infinity,
			update: () => {
				ctx.clearRect(0, 0, canvasEl.width, canvasEl.height);
				updateParticles();
			}
		});

		window.addEventListener('resize', setCanvasSize);

		return () => {
			window.removeEventListener('resize', setCanvasSize);
		};
	});
</script>

<main class=" {showIntro || showPassword ? 'overflow-hidden h-[100dvh]' : ''} ">
	{#if showPassword}
		<div id="password" class="h-[100dvh] flex flex-col justify-center items-center">
			<canvas bind:this={canvasEl} class="absolute top-0 left-0 -z-10 bg-blue-100"></canvas>

			<form
				on:submit|preventDefault={() => {
					if ((password = 'treatyoself')) {
						showPassword = false;
						showIntro = true;
						animation.pause();
					}
				}}
				class="flex z-10"
			>
				<Input class="w-9/12 mr-2" placeholder="Enter your password" bind:value={password}></Input>
				<Button size="icon">
					<ArrowRight></ArrowRight>
				</Button>
			</form>
		</div>
	{/if}
	{#if !showPassword}
		<div id="intro" class=" h-[100dvh] flex flex-col bg-teal-50">
			<Typewriter
				mode={'cascade'}
				interval={30}
				delay={500}
				on:done={() => {
					showButton = true;
					showIntro = false;
				}}
			>
				<div class="my-10 mx-8">
					<h1 class=" capitalize py-4 text-3xl font-semibold">Hello Kexin Zhang!</h1>
					<p class="whitespace-break-spaces">{' '}</p>
					<p>
						I was originally going to get you a gift card for food delivery since you need tasty
						convenient brain fuel for your art doctor dreams! But all the delivery services are
						actually so bad for restaurants.
					</p>
					<p class="whitespace-break-spaces">{' '}</p>
					<p>So I made this bc I didn't want support ubereats lol</p>
					<p class="whitespace-break-spaces">{' '}</p>
					<p>
						Turns out your local restaurants do not have their own delivery, so this is more like a
						general list of places near you but I tried...
					</p>
					<p class="whitespace-break-spaces">{' '}</p>
					<p>Hope you like the restaurants!</p>
				</div>
			</Typewriter>
			<button
				class="duration-1000 transition-transform self-center mt-auto {showButton
					? '-translate-y-0'
					: 'translate-y-full'}"
				on:click={() => {
					mainIntoView('resies');
				}}
			>
				<div class="flex flex-col items-center">
					<p>To the Restaurants</p>
					<ChevronDown size={64} strokeWidth={1} color="#111"></ChevronDown>
				</div>
			</button>
		</div>
	{/if}

	{#if !showIntro && !showPassword}
		<div class="inline-flex absolute rounded-md right-1/2 translate-x-1/2 z-10 my-6" role="group">
			<button
				on:click={() => {
					showMap = false;
				}}
				type="button"
				class="font-['Sniglet'] py-2 text-md font-bold rounded-s-lg border border-black {showMap
					? ' bg-white w-[30vw]'
					: ' bg-yellow-100 w-[43vw]'} duration-300 shadow-md"
			>
				List
			</button>

			<button
				on:click={() => {
					showMap = true;
					mainIntoView('resies');
				}}
				type="button"
				class=" font-['Sniglet'] font-bold border-b border-t border-r border-black px-16 py-2 text-sm rounded-e-lg shadow-md {!showMap
					? ' bg-white w-[43vw]'
					: ' bg-yellow-100 w-[30vw]'} duration-300"
			>
				Map
			</button>
		</div>
	{/if}

	<div id="resies bg-teal-50">
		{#if showMap}
			<div id="map">
				<Map></Map>
			</div>
		{:else}
			<div class="mx-[7vw] pt-20 pb-8">
				<div class="my-2">
					<DropdownMenu.Root>
						<DropdownMenu.Trigger asChild let:builder>
							<Button
								variant="outline"
								builders={[builder]}
								class="bg-purple-100 border font-['Sniglet']  border-black w-[33vw] capitalize"
								>{filter}</Button
							>
						</DropdownMenu.Trigger>
						<DropdownMenu.Content class="w-[33vw]">
							{#each resFilters as res}
								<DropdownMenu.Item
									on:click={() => {
										filter = res;
									}}
									class="capitalize font-['Sniglet'] ">{res}</DropdownMenu.Item
								>
							{/each}
						</DropdownMenu.Content>
					</DropdownMenu.Root>
				</div>
				{#each restaurants.filter((r) => filter == 'all' || r.cuisine == filter) as restaurant}
					<Restaurant res={restaurant}></Restaurant>
				{/each}
			</div>
		{/if}
	</div>
</main>
