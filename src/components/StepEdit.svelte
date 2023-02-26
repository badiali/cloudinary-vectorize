<script lang="ts">
	import "two-up-element";
	import { originalImage, modifiedImage } from "../store";

	let processingImage = true;
	let tries = 0;
	let intervalId: any;

	$: {
		if (processingImage) {
			clearInterval(intervalId);
			intervalId = setInterval(() => {
				tries++;
				const img = new Image();
				img.src = $modifiedImage;
				img.onload = () => {
					processingImage = false;
					clearInterval(intervalId);
				};
			}, 500);
		}
	}
</script>

<div class="container flex-col">
	<two-up class="max-w-xl mx-auto">
		<!-- svelte-ignore a11y-img-redundant-alt -->
		<img
			src={$originalImage}
			class="w-full"
			alt="Original image uploaded by user"
		/>
		{#if processingImage}
			<div class="flex flex-col justify-center items-center">
				<p class="text-center m-0 font-bold">Processing image...</p>
			</div>
		{:else}
			<!-- svelte-ignore a11y-img-redundant-alt -->
			<img
				src={$modifiedImage}
				alt="Vectorized image uploaded by user"
			/>
		{/if}
	</two-up>
	<a
		href={$modifiedImage}
		download="vectorized-image.svg"
		class="block btn font-bold bg-blue-600 rounded-full text-bold text-white text-xl text-center px-6 py-4 mt-10 mx-auto max-w-md"
	>
		Download vectorized image
	</a>
</div>
