<script>
	import { fly, fade } from 'svelte/transition';
	let visible = true;
	let names = [
		'Abdi H.',
		'Amanda C.',
		'Antara S.',
		'Charles J.',
		'Christine C.',
		'Dandre D.',
		'David B.',
		'Don M.',
		'Echeta O.',
		'Enshen Z.',
		'Evan B.',
		'Harris G.',
		'Kaitlyn S.',
		'Kelcie M.',
		'Kenneth W.',
		'Kiranjot K.',
		'Luca V.',
		'Megha P.',
		'Michael K.',
		'Michael P.',
		'Mohammad A.',
		'Oleg D.',
		'Oleksandr A.',
		'Rekha B.',
		'Sevan B.',
		'Somaiah U.',
		'Sophie Q.',
		'Usama P.',
		'Vijay R.',
		'William C.'
	];
	let availableNames = names;
	let volunteerOne = 'One';
	let volunteerTwo = 'Two';
	let volunteerThree = 'Three';
	let participantsLeft = availableNames.length;

	function getVolunteers(duration, skipSpeed) {
		visible = visible ? !visible : visible;
		// make sure that we always have volunteers to choose from
		if (participantsLeft < 4) {
			availableNames = names;
		}

		function getName() {
			return availableNames[Math.floor(Math.random() * availableNames.length)];
		}

		let timerId = setInterval(() => {
			volunteerOne = getName();
			volunteerTwo = getName();
			volunteerThree = getName();
		}, skipSpeed);

		// stop after 1 second
		setTimeout(() => {
			clearInterval(timerId);
			removeFromNames();
		}, duration);

		// function to display random names and later set the final three
		function setNames() {
			volunteerOne = getName();
			volunteerTwo = getName();
			volunteerThree = getName();
		}

		// select three unique volunteers and remove them from the list
		function removeFromNames() {
			volunteerOne = getName();
			volunteerTwo = volunteerTwo === volunteerOne ? getName() : volunteerTwo;
			volunteerThree =
				volunteerThree === volunteerTwo || volunteerThree === volunteerOne
					? getName()
					: volunteerThree;
			availableNames = availableNames.filter(
				(i) => i !== volunteerOne && i !== volunteerTwo && i != volunteerThree
			);
			participantsLeft = availableNames.length;
		}

		// TODO: Have and array to store the volunteers.  will enable selecting different number of volunteers and will clean up the code
	}
</script>

<main>
	<h1>PARTICIPATRON</h1>
	<ul>
		<div class="card">
			<img src={`https://robohash.org/${volunteerOne}.png`} alt={`robot from ${volunteerOne}`} />
			<li class="chosen">{volunteerOne}</li>
		</div>
		<div class="card">
			<img src={`https://robohash.org/${volunteerTwo}.png`} alt={`robot from ${volunteerTwo}`} />
			<li class="chosen">{volunteerTwo}</li>
		</div>
		<div class="card">
			<img src={`https://robohash.org/${volunteerThree}.png`} alt={`robot from ${volunteerTwo}`} />
			<li class="chosen">{volunteerThree}</li>
		</div>
	</ul>

	<button on:click={() => getVolunteers(500, 100)}><div class="flower">&#10044;</div></button>
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300&family=Nunito+Sans:wght@200&display=swap');
	:root {
		background: #eef0f4;
		height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	main {
		max-width: 70vw;
		font-family: 'Nunito Sans', sans-serif;
		color: rgb(161, 161, 161);
		display: flex;
		flex-direction: column;
		align-items: center;
		background: #eef0f4;
		border-radius: 5%;
		box-shadow: inset 9.91px 9.91px 15px #d9dade, inset -9.91px -9.91px 15px #ffffff;
		padding: 5rem 4rem;

		/* box-shadow: inset 9.91px 9.91px 15px #d9dade, inset 0px 0px 19px 8px rgba(255, 105, 180, 0.65); */
	}

	h1 {
		font-family: 'Lato', sans-serif;
		margin-top: 0;
		text-decoration: underline;
		text-decoration-thickness: 0.2em;
		text-decoration-color: #ff69b4;
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
		background: #eef0f4;
		border-radius: 100%;
		box-shadow: 9.91px 9.91px 15px #d9dade, -9.91px -9.91px 15px #ffffff;
		overflow: hidden;
		border: 5px solid #fff;
	}

	ul {
		padding: 0;
		margin: 0;
		padding-inline-start: 0;
	}
	li {
		font-size: 1.5rem;
		background: #eef0f4;
		border-radius: 5%;
		box-shadow: inset 9.91px 9.91px 15px #d9dade, inset -9.91px -9.91px 15px #ffffff;
		padding: 1.5rem 1rem;
		list-style: none;
		min-width: 60vw;
		text-align: center;
	}

	i {
		font-style: normal;
	}
	button {
		width: 10rem;
		height: 10rem;
		background: #eef0f4;
		border-radius: 100%;
		box-shadow: 9.91px 9.91px 15px #d9dade, -9.91px -9.91px 15px #ffffff;
		font-family: 'Nunito Sans', sans-serif;
		font-size: 4rem;
		color: #8a8888;
		border: none;
	}

	button:hover {
		border: 3px solid white;
		color: #aaa9a9;
		outline: 1px solid white;
	}

	button:focus {
		outline: 1px solid white;
	}

	button:active {
		box-shadow: 4px 4px 12px #c5c5c5, -4px -4px 12px #ffffff;
		outline: 1px solid white;
	}
	button:active .flower {
		animation-name: spin;
		animation-duration: 2s;
		animation-timing-function: linear;
		animation-iteration-count: infinite;
		outline: none;
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
