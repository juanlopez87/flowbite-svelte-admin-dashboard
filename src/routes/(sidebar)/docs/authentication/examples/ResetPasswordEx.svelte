<script lang="ts">
	import { Label, Input } from 'flowbite-svelte';
	import { ResetPassword } from '$lib';

	const title = 'Reset your password';
	const site = {
		name: 'Flowbite',
		img: '/images/flowbite-svelte-icon-logo.svg',
		link: '/',
		imgAlt: 'FlowBite Logo',
	};
	const acceptTerms = true;
	const btnTitle = 'Create account';
	const termsLink = '/';
	const labelClass = 'mb-2 dark:text-white';
	const inputClass = 'border outline-none dark:border-gray-600 dark:bg-gray-700';
	const onSubmit = (e: Event) => {
		const formData = new FormData(e.target as HTMLFormElement);

		const data: Record<string, string | File> = {};
		for (const field of formData.entries()) {
			const [key, value] = field;
			data[key] = value;
		}
		console.log(data);
	};
</script>

<ResetPassword {title} {site} {acceptTerms} {btnTitle} {termsLink} onsubmit={onSubmit}>
	<div>
		<Label for="email" class={labelClass}>Your email</Label>
		<Input
			type="email"
			name="email"
			id="email"
			placeholder="name@company.com"
			required
			class={inputClass}
		/>
	</div>
	<div>
		<Label for="password" class={labelClass}>Your password</Label>
		<Input
			type="password"
			name="password"
			id="password"
			placeholder="••••••••"
			required
			class={inputClass}
		/>
	</div>
	<div>
		<Label for="confirm-password" class={labelClass}>Confirm password</Label>
		<Input
			type="password"
			name="confirm-password"
			id="confirm-password"
			placeholder="••••••••"
			required
			class={inputClass}
		/>
	</div>
</ResetPassword>
