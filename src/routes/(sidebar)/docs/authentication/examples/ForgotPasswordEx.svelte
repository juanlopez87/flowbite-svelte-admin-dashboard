<script lang="ts">
	import { ForgotPassword } from '$lib';
	import { Label, Input, Checkbox, A } from 'flowbite-svelte';

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

<ForgotPassword onsubmit={onSubmit}>
	<div>
		<Label for="email" class="mb-2">Your email</Label>
		<Input
			type="email"
			name="email"
			id="email"
			placeholder="name@company.com"
			required
			class="border outline-none"
		/>
	</div>
	<Checkbox class="gap-1">I accept the <A href="/">Terms and Conditions</A></Checkbox>
</ForgotPassword>
