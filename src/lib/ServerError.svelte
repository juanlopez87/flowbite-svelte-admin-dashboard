<script lang="ts">
	import { imagesPath } from './variables';
	import { twMerge } from 'tailwind-merge';
	import { Button, P } from 'flowbite-svelte';
	import type { ServerErrorProps } from './types';

	let {
		title = 'Something has gone seriously wrong',
		description = "It's always time for a coffee break. We should be back by the time you finish your coffee.",
		image,
		btnTitle = 'Go back home',
		btnHref = '/',
		mainClass = 'bg-gray-50 dark:bg-gray-900',
		mainDivClass,
		h1Class,
		imgDiv = 'block md:max-w-lg',
		div2Class = 'text-center xl:max-w-4xl',
	}: ServerErrorProps = $props();

	const imageDefault = {
		src: imagesPath('illustrations/500.svg'),
		alt: 'astronaut',
	};
	const imageOptions = $derived(image ?? imageDefault);

	const mainDivCls = twMerge(
		'flex flex-col justify-center items-center px-6 mx-auto h-screen xl:px-0 dark:bg-gray-900',
		mainDivClass,
	);
	const h1Cls = twMerge(
		'mb-3 text-2xl font-bold leading-tight text-gray-900 sm:text-4xl lg:text-5xl dark:text-white',
		h1Class,
	);
</script>

<main class={mainClass}>
	<div class={mainDivCls}>
		<div class={imgDiv}>
			<img src={imageOptions.src} alt={imageOptions.alt} />
		</div>
		<div class={div2Class}>
			<h1 class={h1Cls}>
				{title}
			</h1>
			<P
				class="mb-5 text-base font-normal text-gray-500 md:text-lg dark:text-gray-300"
			>
				{description}
			</P>
			<Button href={btnHref}>
				<svg
					class="mr-2 -ml-1 h-5 w-5"
					fill="currentColor"
					viewBox="0 0 20 20"
					xmlns="http://www.w3.org/2000/svg"
					><path
						fill-rule="evenodd"
						d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
						clip-rule="evenodd"
					/></svg
				>
				{btnTitle}
			</Button>
		</div>
	</div>
</main>

<!--
@component
[Go to docs](https://flowbite-svelte-admin-dashboard.vercel.app/)
## Type
[ServerErrorProps](https://github.com/themesberg/flowbite-svelte-admin-dashboard/blob/main/src/lib/types.ts#L214)
## Props
@prop title = 'Something has gone seriously wrong'
@prop description = "It's always time for a coffee break. We should be back by the time you finish your coffee."
@prop image
@prop btnTitle = 'Go back home'
@prop btnHref = '/'
@prop mainClass = 'bg-gray-50 dark:bg-gray-900'
@prop mainDivClass
@prop h1Class
@prop imgDiv = 'block md:max-w-lg'
@prop div2Class = 'text-center xl:max-w-4xl'
-->
