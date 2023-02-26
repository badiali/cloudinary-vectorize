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
			<img
				src="https://res.cloudinary.com/badiali/image/upload/e_vectorize:3:0.9/f_svg/v1677139869/cld-sample-5.jpg"
				class="w-full mb-8 rounded-[2rem]"
				alt="Example image"
			/>
			<h1
				class="font-bold tracking-tight mb-3 text-4xl md:text-5xl lg:text-6xl text-center lg:!text-left"
			>
				<span class="text-gradient">Vectorize</span> Image
			</h1>
			<p class="font-bold text-xl m-0 text-center lg:!text-left">
				100% Automatically and
				<span class="text-gradient"> Free</span>
			</p>
		</div>
		<div class="relative flex flex-col gap-4 md:gap-8 max-w-md">
			<StepUpload />
		</div>
	</div>
{:else if $imageStatus === ImageStatus.DONE}
	<StepEdit />
{/if}
