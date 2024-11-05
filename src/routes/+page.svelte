<script lang="ts">
	import { createCodepointNameMap } from '$lib';

	let dots = $state(new Array(8).fill(false));
	let codepoint = $derived.by(() => {
		let braille = 0;
		for (let i = 0; i < 8; i++) {
			if (dots[i]) {
				braille |= 1 << i;
			}
		}
		return braille + 0x2800;
	});
	let braille = $derived(String.fromCodePoint(codepoint));

	let codepointNameMap: Map<number, string> = createCodepointNameMap();
</script>

<h1>braille tool</h1>
<div class="main">
	<div class="brailleBlock">
		<div class="brailleRow">
			<div class="brailleColumn">
				<input type="checkbox" bind:checked={dots[0]} />
				<input type="checkbox" bind:checked={dots[1]} />
				<input type="checkbox" bind:checked={dots[2]} />
			</div>
			<div class="brailleColumn">
				<input type="checkbox" bind:checked={dots[3]} />
				<input type="checkbox" bind:checked={dots[4]} />
				<input type="checkbox" bind:checked={dots[5]} />
			</div>
		</div>

		<div class="brailleRow">
			<input type="checkbox" bind:checked={dots[6]} />
			<input type="checkbox" bind:checked={dots[7]} />
		</div>
	</div>
	<div class="displayBlock">
		<div class="displayRow">
			<button>copy</button>
			<span class="brailleDisplay">{braille}</span>
		</div>
		<span class="description"
			>U+{codepoint.toString(16).toUpperCase()} {codepointNameMap?.get(codepoint)}</span
		>
	</div>
</div>

<style>
	.main {
		display: flex;
		flex-direction: column;
	}
	.brailleBlock {
		display: flex;
		flex-direction: column;
	}

	.brailleRow {
		display: flex;
		flex-direction: row;
	}

	.brailleColumn {
		display: flex;
		flex-direction: column;
	}

	span.brailleDisplay {
		font-size: 4rem;
		background-color: #00000022;
	}

	.displayBlock {
		display: flex;
		flex-direction: column;
	}

	.displayRow {
		display: flex;
		flex-direction: row;
	}

	span.description {
		font-size: 1rem;
		font-family: monospace;
	}

	input {
		width: 20px;
		height: 20px;
	}
</style>
