<script lang="ts">
	import { names } from '../names';

	let availableNames: string[] = names;
	let participantsLeft: number = availableNames.length;
	let namesToDisplay: string[] = ['0101', '0001', '1001'];
	let volunteers: number = 3;

	function getVolunteers(duration: number, skipSpeed: number, number_volunteers: number) {
		//make sure that we always have volunteers to choose from
		if (participantsLeft < 4) {
			availableNames = names;
		}

		// returns a random volunteer
		function getName(): string {
			const foundVolunteer = availableNames[Math.floor(Math.random() * availableNames.length)];
			return foundVolunteer;
		}

		// gets as many volunteers as chosen and adds the names to the namesTodisplay array
		let timerId = setInterval((): void => {
			namesToDisplay = [];
			for (let i = 0; i < number_volunteers; i++) {
				namesToDisplay.push(getName());
				namesToDisplay = namesToDisplay;
			}
		}, skipSpeed);

		// stop after chosen duration in getVolunteers Function (set at 1 second)
		setTimeout((): void => {
			clearInterval(timerId);
			removeFromNames();
		}, duration);

		// select three unique volunteers and removes them from the list for the next selection round
		function removeFromNames(): void {
			namesToDisplay = [];
			for (let i = 0; i < number_volunteers; i++) {
				const foundVolunteer = getName();
				namesToDisplay.push(foundVolunteer);
				namesToDisplay = namesToDisplay;
				availableNames = availableNames.filter((i) => i !== foundVolunteer);
			}
			participantsLeft = availableNames.length;
		}
	}
</script>

<svelte:head>
	<title>Participatron</title>
</svelte:head>

<main>
	<h1>PARTICIPATRON</h1>
	<ul>
		{#each namesToDisplay as name}
			<div class="card">
				<img src={`https://robohash.org/${name}.png`} alt={`robot from ${name}`} />
				<li class="chosen">{name}</li>
			</div>
		{/each}
	</ul>
	<button on:click={() => getVolunteers(1000, 100, volunteers)}
		><div class="flower">&#10044;</div></button
	>
	<section>
		<input type="range" bind:value={volunteers} min="1" max="6" />
		<div class="volunteers">{volunteers} <label for="thinking">volunteers</label></div>
	</section>
</main>

<style>
	/* FONTS USED */
	@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300&family=Nunito+Sans:wght@200&display=swap');

	:root {
		/* VARIABLES */
		--font-color: #a1a1a1;
		--back-color: #eef0f4;
		--decoration-color: #ff69b4;
		--decoration-pure: #ffffff;
		--button-text-color: #8a8888;

		background: var(--back-color);
		height: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		overflow-y: scroll;
		margin-top: 1rem;
	}

	main {
		height: 100%;
		max-width: 70vw;
		font-family: 'Nunito Sans', sans-serif;
		color: var(--font-color);
		display: flex;
		flex-direction: column;
		align-items: center;
		background: var(--back-color);
		border-radius: 5%;
		box-shadow: inset 9.91px 9.91px 15px #d9dade, inset -9.91px -9.91px 15px var(--decoration-pure);
		padding: 5rem 4rem;
	}

	h1 {
		font-family: 'Lato', sans-serif;
		margin-top: 0;
		text-decoration: underline;
		text-decoration-thickness: 0.2em;
		text-decoration-color: var(--decoration-color);
		position: sticky;
	}

	ul {
		padding: 0;
		margin: 0;
		padding-inline-start: 0;
	}

	li {
		font-size: 1.5rem;
		background: var(--back-color);
		border-radius: 5%;
		box-shadow: inset 9.91px 9.91px 15px #d9dade, inset -9.91px -9.91px 15px var(--decoration-pure);
		padding: 1.5rem 1rem;
		list-style: none;
		min-width: 60vw;
		text-align: center;
	}

	.card {
		display: flex;
		position: relative;
		margin: 3rem 0;
	}

	.card img {
		position: absolute;
		top: -10px;
		left: -30px;
		max-height: 6rem;
		background: var(--back-color);
		border-radius: 100%;
		box-shadow: 9.91px 9.91px 15px #d9dade, -9.91px -9.91px 15px var(--decoration-pure);
		border: 5px solid var(--boder-color);
	}

	button {
		width: 10rem;
		height: 10rem;
		background: var(--back-color);
		border-radius: 100%;
		box-shadow: 9.91px 9.91px 15px #d9dade, -9.91px -9.91px 15px var(--decoration-pure);
		font-family: 'Nunito Sans', sans-serif;
		font-size: 4rem;
		color: var(--button-text-color);
		border: none;
	}

	button:hover {
		border: 3px solid var(--decoration-pure);
		color: #aaa9a9;
		outline: 1px solid var(--decoration-pure);
	}

	button:focus {
		outline: 1px solid var(--decoration-pure);
	}

	button:active {
		box-shadow: 4px 4px 12px #c5c5c5, -4px -4px 12px #ffffff;
		outline: 1px solid var(--decoration-pure);
	}
	button:active .flower {
		animation-name: spin;
		animation-duration: 2s;
		animation-timing-function: linear;
		animation-iteration-count: infinite;
		outline: none;
	}

	.volunteers {
		margin-top: 1rem;
		text-align: center;
	}

	/* part From CSS Tricks */
	input[type='range'] {
		margin-top: 2rem;
		-webkit-appearance: none;
		width: 100%;
		background: transparent;
	}
	input[type='range']::-webkit-slider-thumb {
		-webkit-appearance: none;
	}

	input[type='range']:focus {
		outline: 1px solid var(--decoration-pure);
	}

	input[type='range']::-ms-track {
		width: 100%;
		cursor: pointer;

		background: transparent;
		border-color: transparent;
		color: transparent;
	}
	input[type='range']::-webkit-slider-thumb {
		-webkit-appearance: none;
		height: 2rem;
		width: 2rem;
		border-radius: 50%;
		background: var(--decoration-color);
		cursor: pointer;
		margin-top: 0px;
		box-shadow: 1px 1px 1px #c5c5c5, 0px 0px 1px var(--decoration-pure);
	}

	input[type='range']::-webkit-slider-runnable-track {
		width: 100%;
		height: 2rem;
		cursor: pointer;
		border-radius: 5%;
		box-shadow: inset 10.51px 10.51px 9px #e1e3e7, inset -10.51px -10.51px 9px #fbfdff;
	}

	@keyframes spin {
		0% {
			transform: rotate(0deg);
		}
		50% {
			transform: rotate(1200deg);
		}
		100% {
			transform: rotate(2400deg);
		}
	}

	@media screen and (max-width: 600px) {
		li {
			text-align: right;
		}
	}
</style>
