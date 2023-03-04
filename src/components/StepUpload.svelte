<script lang="ts">
	import { Cloudinary } from "@cloudinary/url-gen";
	import { vectorize } from "@cloudinary/url-gen/actions/effect";

	import { ImageStatus } from "../types.d";
	import { imageStatus, modifiedImage, originalImage } from "../store";
	import Dropzone from "dropzone";
	import "dropzone/dist/dropzone.css";

	import { onMount } from "svelte";

	const cloudinary = new Cloudinary({
		cloud: {
			cloudName: "badiali",
		},
		url: {
			secure: true,
		},
	});

	onMount(() => {
		const dropzone = new Dropzone("#dropzone", {
			uploadMultiple: false,
			acceptedFiles: ".jpg,.jpeg,.png,.webp",
			maxFiles: 1,
			thumbnailWidth: 200,
			thumbnailHeight: 120,
		});
		const apiKey = process.env.CLOUDINARY_API_KEY;

		dropzone.on("sending", (file, xhr, formData) => {
			imageStatus.set(ImageStatus.UPLOADING);
			formData.append("upload_preset", "ml_default");
			formData.append("timestamp", Date.now() / 1000);
			formData.append("api_key", { apiKey });
		});

		dropzone.on("success", (file, response) => {
			const { public_id: publicId, secure_url: url } = response;

			const vectorizedImage = cloudinary
				.image(publicId)
				.effect(
					vectorize()
						/*	The number of colors.
							Range: 2 to 30. Default: 10. */
						.numOfColors(10)
						/* The level of detail, either as a percentage of the original image, or an absolute number of pixels.
							Range: 0.0 to 1.0, or 0 to 1000. Default: 300. */
						.detailsLevel(500)
						/*	The size up to which to suppress speckles, either as a percentage of the original image, or an absolute number of pixels.
							Range: 0.0 to 1.0, or 0 to 100. Default: 2. */
						.despeckleLevel(200)
						/*	The Bezier curve optimization value up to 100 for least optimization and the largest file.
							Range: 0 to 100. Default: 100. */
						.paths(50)
						/* The corner threshold parameter. The lower the value, the smoother the corners.
							Range: 0 to 100. Default: 25. */
						.cornersLevel(50)
				)
				.format("svg");

			imageStatus.set(ImageStatus.DONE);
			originalImage.set(url);
			modifiedImage.set(vectorizedImage.toURL());
		});

		dropzone.on("error", (file, response) => {
			console.log("BAD");
			console.log(response);
		});
	});
</script>

<form
	id="dropzone"
	class="dropzone-form w-full flex flex-col sm:justify-center sm:items-center sm:gap-8 sm:pt-36 sm:pb-16 sm:px-16 rounded-[2rem]"
	action="https://api.cloudinary.com/v1_1/badiali/image/upload"
>
	{#if $imageStatus === ImageStatus.READY}
		<button
			class="btn font-bold pointer-events-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-4"
		>
			Upload file
		</button>
		<strong class="hidden sm:block text-lg mt-4 text-gray-800"
			>or drop a file</strong
		>
	{:else if $imageStatus === ImageStatus.UPLOADING}
		<strong class="text-md p-3 mt-4 text-gray-800">uploading file...</strong
		>
	{/if}
</form>

<style>
	.dropzone-form * {
		position: relative;
		z-index: 2;
	}
	.dropzone-form::before {
		content: "";
		position: absolute;
		top: 0;
		left: 0;
		z-index: 1;
		width: 100%;
		height: 100%;
		border-radius: 2rem;
		background-color: #eef2ff;
	}
	.dropzone-form::after {
		content: "";
		position: absolute;
		top: 1.5rem;
		left: -1.5rem;
		z-index: 0;
		width: 100%;
		height: 100%;
		border-radius: 2rem;
		background-image: radial-gradient(currentColor 1px, transparent 1px);
		background-size: 10px 10px;
		color: var(--primary);
	}
	@media (min-width: 1024px) {
		.dropzone-form::after {
			top: 2rem;
			left: -2rem;
		}
	}
</style>
