<script lang="ts">
	import { createEventDispatcher, onMount } from "svelte";

	const dispatch = createEventDispatcher();

	export let messagePairIndex: number;
	let isVisible = true;
	let chosenOption = "";

	const RESPONSE_A_INDEX = 1;
	const RESPONSE_B_INDEX = 2;

	let buttonValues = [
		{
			label: "A",
			score: 1,
			indexToDelete: RESPONSE_B_INDEX,
			fontSize: 30,
			css: "rating-button option-a"
		},
		{
			label: "A",
			score: 2,
			indexToDelete: RESPONSE_B_INDEX,
			fontSize: 24,
			css: "rating-button option-a"
		},
		{
			label: "A",
			score: 3,
			indexToDelete: RESPONSE_B_INDEX,
			fontSize: 18,
			css: "rating-button option-a"
		},
		{
			label: "A",
			score: 4,
			indexToDelete: RESPONSE_B_INDEX,
			fontSize: 12,
			css: "rating-button option-a"
		},
		{
			label: "B",
			score: 5,
			indexToDelete: RESPONSE_A_INDEX,
			fontSize: 12,
			css: "rating-button option-b"
		},
		{
			label: "B",
			score: 6,
			indexToDelete: RESPONSE_A_INDEX,
			fontSize: 18,
			css: "rating-button option-b"
		},
		{
			label: "B",
			score: 7,
			indexToDelete: RESPONSE_A_INDEX,
			fontSize: 24,
			css: "rating-button option-b"
		},
		{
			label: "B",
			score: 8,
			indexToDelete: RESPONSE_A_INDEX,
			fontSize: 30,
			css: "rating-button option-b"
		}
	];

	const sortButtons = () => {
		buttonValues.sort((a, b) => a.fontSize - b.fontSize);
	};

	onMount(sortButtons);

	function handleButtonClick(i: number, j: number, score: number) {
		isVisible = false;
		chosenOption = buttonValues[j].label;
		dispatch("select_response", { i, j, score });
	}
</script>

<div class="rating-bar-container">
	{#if isVisible}
		<div class="rating-bar">
			<div class="rating-button-row">
				{#each buttonValues as button, i}
					<button
						class={button.css}
						style="font-size: {button.fontSize}px;"
						on:click={() =>
							handleButtonClick(
								messagePairIndex,
								button.indexToDelete,
								button.score
							)}
						on:mouseover={() =>
							dispatch(button.label === "A" ? "aHovered" : "bHovered", true)}
						on:mouseout={() =>
							dispatch(button.label === "A" ? "aHovered" : "bHovered", false)}
						on:focus={() =>
							dispatch(button.label === "A" ? "aHovered" : "bHovered", true)}
						on:blur={() =>
							dispatch(button.label === "A" ? "aHovered" : "bHovered", false)}
					>
						<span class="rating-button-label">{button.label}</span>
					</button>
				{/each}
			</div>
		</div>
	{/if}
</div>

<style>
	.rating-bar-container {
		position: relative;
		margin: auto;
	}

	.rating-bar {
		margin: auto;
		background-color: var(--background-shade);
		padding: var(--spacing-xxl);
		font-weight: bold;
		font-size: 16px;
		font-family: var(--font);
	}

	.rating-button-row {
		display: flex;
		align-items: center;
	}

	.rating-button {
		display: flex;
		justify-content: center;
		align-items: center;
		transition: transform 0.2s ease-in-out;
		box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
		border: 1px solid var(--border-color-accent);
		background-color: var(--color-accent-soft);
		padding: 0;
		width: 48px;
		height: 36px;
		font-size: 12px;
	}

	.rating-button:hover {
		transform: translateY(-3px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
		background-color: var(--border-color-accent);
	}

	.rating-button:not(:last-child) {
		border-right: 1px solid var(--border-color-accent);
	}

	.rating-button-label {
		display: inline-block;
		vertical-align: middle;
	}

	.rating-button:first-child {
		border-top-left-radius: 8px;
		border-bottom-left-radius: 8px;
	}

	.rating-button:last-child {
		border-top-right-radius: 8px;
		border-bottom-right-radius: 8px;
	}
</style>
