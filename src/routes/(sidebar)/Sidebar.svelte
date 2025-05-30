<script lang="ts">
	import { afterNavigate } from '$app/navigation';
	import { page } from '$app/state';

	import {
		Sidebar,
		SidebarDropdownWrapper,
		SidebarGroup,
		SidebarItem,
		SidebarWrapper,
		SidebarButton,
		uiHelpers,
	} from 'flowbite-svelte';
	import {
		AngleDownOutline,
		AngleUpOutline,
		ClipboardListSolid,
		CogOutline,
		FileChartBarSolid,
		GithubSolid,
		LayersSolid,
		LifeSaverSolid,
		LockSolid,
		WandMagicSparklesOutline,
		ChartPieOutline,
		RectangleListSolid,
		TableColumnSolid,
		GridSolid,
		FireOutline,
		BookOpenOutline,
	} from 'flowbite-svelte-icons';

	interface Props {
		drawerHidden: boolean;
		docsRoute: string[];
	}
	let { drawerHidden = $bindable(false), docsRoute }: Props = $props();
	// console.log('data in Sidebar docsRoute:', docsRoute)
	const closeDrawer = () => {
		drawerHidden = true;
	};

	let iconClass =
		'flex-shrink-0 w-6 h-6 text-gray-500 transition duration-75 group-hover:text-gray-900 dark:text-gray-300 dark:group-hover:text-white';
	let itemClass =
		'flex items-center p-2 text-base text-gray-900 transition duration-75 rounded-lg hover:bg-gray-100 group dark:text-gray-200 dark:hover:bg-gray-700 w-full';
	let groupClass = 'pt-2 space-y-2 mb-3';

	let activeUrl = $derived(page.url.pathname);
	let activeMainSidebar: string;

	const sidebarUi = uiHelpers();
	let isOpen = $state(false);
	const closeSidebar = sidebarUi.close;
	$effect(() => {
		isOpen = sidebarUi.isOpen;
		activeUrl = page.url.pathname;
	});

	afterNavigate((navigation) => {
		// this fixes https://github.com/themesberg/flowbite-svelte/issues/364
		document.getElementById('svelte')?.scrollTo({ top: 0 });
		closeDrawer();

		activeMainSidebar = navigation.to?.url.pathname ?? '';
	});

	let posts = [
		{ name: 'Dashboard', Icon: ChartPieOutline, href: '/dashboard' },
		{
			name: 'Layouts',
			Icon: TableColumnSolid,
			children: {
				Stacked: '/layouts/stacked',
				Sidebar: '/layouts/sidebar',
			},
		},
		{
			name: 'CRUD',
			Icon: RectangleListSolid,
			children: {
				Products: '/crud/products',
				Users: '/crud/users',
			},
		},
		{ name: 'Settings', Icon: CogOutline, href: '/settings' },
		{
			name: 'Pages',
			Icon: FileChartBarSolid,
			children: {
				Pricing: '/pages/pricing',
				Maintenance: '/errors/400',
				'404 not found': '/errors/404',
				'500 server error': '/errors/500',
			},
		},
		{
			name: 'Authentication',
			Icon: LockSolid,
			children: {
				'Sign in': '/authentication/sign-in',
				'Sign up': '/authentication/sign-up',
				'Forgot password': '/authentication/forgot-password',
				'Reset password': '/authentication/reset-password',
				'Profile lock': '/authentication/profile-lock',
			},
		},
		{
			name: 'Playground',
			Icon: WandMagicSparklesOutline,
			children: {
				Stacked: '/playground/stacked',
				Sidebar: '/playground/sidebar',
			},
		},
	];

	let links = [
		{
			label: 'GitHub Repository',
			href: 'https://github.com/themesberg/flowbite-svelte-admin-dashboard',
			Icon: GithubSolid,
		},
		{
			label: 'Flowbite Svelte',
			href: 'https://flowbite-svelte.com/docs/pages/quickstart',
			Icon: ClipboardListSolid,
		},
		{
			label: 'Components',
			href: 'https://flowbite-svelte.com/docs/components/accordion',
			Icon: LayersSolid,
		},
		{
			label: 'Support',
			href: 'https://github.com/themesberg/flowbite-svelte-admin-dashboard/issues',
			Icon: LifeSaverSolid,
		},
	];
	let dropdowns = Object.fromEntries(Object.keys(posts).map((x) => [x, false]));
</script>

<SidebarButton
	breakpoint="lg"
	onclick={sidebarUi.toggle}
	class="fixed top-[22px] z-40 mb-2"
/>
<Sidebar
	breakpoint="lg"
	backdrop={false}
	{isOpen}
	{closeSidebar}
	params={{ x: -50, duration: 50 }}
	class="top-0 left-0 mt-[69px] h-screen w-64 bg-gray-50 transition-transform lg:block dark:bg-gray-800"
	divClass="h-full px-3 py-4 overflow-y-auto bg-gray-50 dark:bg-gray-800"
	activeClass="p-2"
	nonActiveClass="p-2"
>
	<h4 class="sr-only">Main menu</h4>
	<SidebarWrapper
		divClass="overflow-y-auto px-3 pt-20 lg:pt-5 h-full bg-white scrolling-touch max-w-2xs lg:h-[calc(100vh-4rem)] lg:block dark:bg-gray-800 lg:me-0 lg:sticky top-2"
	>
		<SidebarGroup class={groupClass}>
			{#each posts as { name, Icon, children, href } (name)}
				{#if children}
					<SidebarDropdownWrapper label={name} class="pr-3">
						{#snippet arrowdown()}
							<AngleDownOutline strokeWidth="3.3" size="sm" />
						{/snippet}
						{#snippet arrowup()}
							<AngleUpOutline strokeWidth="3.3" size="sm" />
						{/snippet}
						{#snippet icon()}
							<Icon class={iconClass} />
						{/snippet}
						{#each Object.entries(children) as [title, href]}
							<SidebarItem
								label={title}
								{href}
								spanClass="ml-9"
								class={itemClass}
								aClass="w-full"
							/>
						{/each}
					</SidebarDropdownWrapper>
				{:else}
					<SidebarItem
						label={name}
						{href}
						spanClass="ml-3"
						class={itemClass}
						aClass="w-full p-0 py-2"
					>
						{#snippet icon()}
							<Icon class={iconClass} />
						{/snippet}
					</SidebarItem>
				{/if}
			{/each}
		</SidebarGroup>
		<SidebarGroup class={groupClass}>
			<SidebarDropdownWrapper label="Docs" class="pr-3">
				{#snippet arrowdown()}
					<AngleDownOutline strokeWidth="3.3" size="sm" />
				{/snippet}
				{#snippet arrowup()}
					<AngleUpOutline strokeWidth="3.3" size="sm" />
				{/snippet}
				{#snippet icon()}
					<BookOpenOutline class={iconClass} />
				{/snippet}
				{#each docsRoute as doc}
					<SidebarItem
						label={doc}
						href={`/docs/${doc}`}
						spanClass="ml-3"
						class={itemClass}
					>
						{#snippet icon()}
							<FireOutline
								class="h-5 w-5 text-gray-500 transition duration-75 group-hover:text-gray-900 dark:text-gray-300 dark:group-hover:text-white"
							/>
						{/snippet}
					</SidebarItem>
				{/each}
			</SidebarDropdownWrapper>
			<SidebarItem
				label="About"
				spanClass="flex-1 ms-3 whitespace-nowrap"
				href="/about"
			>
				{#snippet icon()}
					<GridSolid
						class="h-5 w-5 text-gray-500 transition duration-75 group-hover:text-gray-900 dark:text-gray-300 dark:group-hover:text-white"
					/>
				{/snippet}
			</SidebarItem>
		</SidebarGroup>

		<SidebarGroup class={groupClass}>
			{#each links as { label, href, Icon } (label)}
				<SidebarItem
					{label}
					{href}
					spanClass="ml-3"
					class={itemClass}
					target="_blank"
				>
					{#snippet icon()}
						<Icon class={iconClass} />
					{/snippet}
				</SidebarItem>
			{/each}
		</SidebarGroup>
	</SidebarWrapper>
</Sidebar>
