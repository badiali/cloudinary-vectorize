<script lang="ts">
	import { ImageStatus } from "../types.d";
	import { imageStatus } from "../store";
	import StepUpload from "../components/StepUpload.svelte";
	import StepEdit from "../components/StepEdit.svelte";
</script>

{#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
	<div
		class="w-full flex flex-col lg:flex-row items-center justify-between gap-10 md:gap-16 px-6 xl:px-0"
	>
		<div
			class="flex flex-col lg:flex-col items-center lg:items-start md:max-w-3xl lg:max-w-lg"
		>
			<!-- svelte-ignore a11y-media-has-caption -->
			<video class="object-cover w-full aspect-[16:10] mb-5" src="./process.mp4" poster="./poster.jpg" autoplay loop muted></video>
			<h1
				class="font-bold tracking-tight mb-3 text-4xl md:text-5xl lg:text-6xl text-center lg:!text-left"
			>
				<span class="text-gradient">Vectorize</span> Image
			</h1>
			<p class="font-bold text-xl m-0 text-center lg:!text-left mb-2">
				100% Automatically and
				<span class="text-gradient"> Free</span>
			</p>
			<p>By using this service, you can download your raster image as an SVG file, making it easier to edit and use for various design projects.</p>
		</div>
		<div class="relative flex flex-col gap-4 md:gap-8 max-w-md">
			<StepUpload />
		</div>
	</div>
{:else if $imageStatus === ImageStatus.DONE}
	<StepEdit />
{/if}
