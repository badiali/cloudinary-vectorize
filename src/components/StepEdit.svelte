<script lang="ts">
	import "two-up-element";
	import { originalImage, modifiedImage } from "../store";
</script>

<div class="container flex-col">
	<two-up class="max-w-xl mx-auto">
		<img
			src={$originalImage}
			class="w-full"
			alt="Original image uploaded from user"
		/>
		<img src={$modifiedImage} alt="Original image uploaded from user" />
	</two-up>
	<a
		download
		href={$modifiedImage}
		class="block btn font-bold bg-blue-600 rounded-full text-bold text-white text-xl text-center px-6 py-4 mt-10 mx-auto max-w-md"
	>
		Download vectorized image
	</a>
</div>
