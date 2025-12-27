<script>
	import { page } from '$app/stores';
	import '../app.css';

	let { children } = $props();
	let isMobileMenuOpen = $state(false);

	const navItems = [
		{ label: 'Home', href: '/' },
		{ label: 'Promo Codes', href: '/promos', badge: 'Hot' },
		{ label: 'Free Cases', href: '/cases' },
		{ label: 'Top Sites', href: '/sites' }
	];

	function toggleMobileMenu() {
		isMobileMenuOpen = !isMobileMenuOpen;
	}

	function closeMobileMenu() {
		isMobileMenuOpen = false;
	}

	let currentYear = new Date().getFullYear();

	let onlineUsers = $state(142);

	$effect(() => {
		const interval = setInterval(() => {
			// Randomly fluctuate between -3 and +5 users
			onlineUsers += Math.floor(Math.random() * 9) - 3;
			if (onlineUsers < 110) onlineUsers = 110;
			if (onlineUsers > 350) onlineUsers = 350;
		}, 3000);

		return () => clearInterval(interval);
	});
</script>

<svelte:head>
	<title>CS2 Giveaways | Win Free Skins, Cases & Promo Codes Daily</title>
	<meta
		name="description"
		content="Join CS2 Giveaways to win free CS2 skins, open daily free cases, and get exclusive promo codes for top CS2 sites. Spin the wheel and claim your rewards!"
	/>
	<meta
		name="keywords"
		content="CS2 giveaways, free CS2 skins, CS2 cases, CS2 promo codes, free skin sites, CSGO skins, win skins"
	/>
	<meta name="robots" content="index, follow" />
	<meta name="author" content="CS2 Giveaways" />

	<!-- Canonical URL -->
	<link
		rel="canonical"
		href="https://cs2giveaways.com{$page.url.pathname === '/' ? '' : $page.url.pathname}"
	/>

	<!-- Open Graph / Facebook -->
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://cs2giveaways.com{$page.url.pathname}" />
	<meta property="og:title" content="CS2 Giveaways | Win Free Skins, Cases & Promo Codes Daily" />
	<meta
		property="og:description"
		content="Join CS2 Giveaways to win free CS2 skins, open daily free cases, and get exclusive promo codes for top CS2 sites. Spin the wheel and claim your rewards!"
	/>
	<meta property="og:image" content="https://cs2giveaways.com/logo.png" />
	<meta property="og:site_name" content="CS2 Giveaways" />
	<meta property="og:locale" content="en_US" />

	<!-- Twitter -->
	<meta property="twitter:card" content="summary_large_image" />
	<meta property="twitter:url" content="https://cs2giveaways.com{$page.url.pathname}" />
	<meta
		property="twitter:title"
		content="CS2 Giveaways | Win Free Skins, Cases & Promo Codes Daily"
	/>
	<meta
		property="twitter:description"
		content="Join CS2 Giveaways to win free CS2 skins, open daily free cases, and get exclusive promo codes for top CS2 sites. Spin the wheel and claim your rewards!"
	/>
	<meta property="twitter:image" content="https://cs2giveaways.com/og-image.jpg" />

	<link rel="icon" href="/favicon.png" />
	{@html `<script type="application/ld+json">
	{
		"@context": "https://schema.org",
		"@type": "Organization",
		"name": "CS2 Giveaways",
		"url": "https://cs2giveaways.com",
		"logo": "https://cs2giveaways.com/logo.png",
		"sameAs": [
			"https://x.com/cs2giveawayspro"
		],
		"description": "The premier destination for CS2 skins, daily free cases, and verified promo codes."
	}
	</script>`}
</svelte:head>

<div
	class="app-container flex min-h-screen flex-col bg-[#0f1115] font-sans text-gray-300 selection:bg-yellow-500/30 selection:text-yellow-200"
>
	<!-- Top Bar -->
	<div
		class="hidden border-b border-white/5 bg-[#1a1d24] py-1 text-center text-xs font-medium text-gray-400 md:block"
	>
		<span class="mx-2"><span class="text-green-400">●</span> New Codes Added Daily</span>
		<span class="mx-2 text-gray-600">|</span>
		<span class="mx-2"><span class="text-yellow-500">★</span> Up-to-date Daily Giveaways</span>
	</div>

	<!-- Header -->
	<header
		class="sticky top-0 z-50 w-full border-b border-white/5 bg-[#0f1115]/80 backdrop-blur-md supports-[backdrop-filter]:bg-[#0f1115]/60"
	>
		<div class="container mx-auto flex h-20 max-w-6xl items-center justify-between px-4">
			<!-- Logo -->
			<a href="/" class="group flex items-center gap-3" onclick={closeMobileMenu}>
				<div class="relative flex h-11 w-11 shrink-0 items-center justify-center">
					<img
						src="/logo.png"
						alt="CS2 Giveaways Logo"
						width="44"
						height="44"
						class="h-full w-full object-contain opacity-90 transition-opacity group-hover:opacity-100"
					/>
				</div>
				<div class="flex flex-col">
					<span
						class="font-display text-2xl leading-none font-bold tracking-wide text-white transition-colors group-hover:text-yellow-400"
					>
						CS2<span class="text-yellow-500">GIVEAWAYS</span>
					</span>
					<span class="text-xs font-semibold tracking-widest text-gray-500 uppercase"
						>Win Skins Daily</span
					>
				</div>
			</a>

			<!-- Live Visitors -->
			<div
				class="ml-6 hidden items-center gap-2 rounded-full border border-green-500/20 bg-green-500/5 px-3 py-1.5 text-xs font-bold text-green-400 md:flex"
			>
				<span class="relative flex h-2 w-2">
					<span
						class="absolute inline-flex h-full w-full animate-ping rounded-full bg-green-400 opacity-75"
					></span>
					<span class="relative inline-flex h-2 w-2 rounded-full bg-green-500"></span>
				</span>
				<span class="text-white">{onlineUsers}</span>
				<span class="font-medium text-gray-500">Online</span>
			</div>

			<!-- Desktop Nav -->
			<nav class="hidden items-center gap-1 lg:flex">
				{#each navItems as item}
					<a
						href={item.href}
						class="relative rounded-md px-4 py-2 text-sm font-medium transition-all duration-200
						{$page.url.pathname === item.href
							? 'bg-white/10 text-white'
							: 'text-gray-400 hover:bg-white/5 hover:text-white'}"
					>
						{item.label}
						{#if item.badge}
							<span
								class="absolute -top-1 -right-1 flex h-4 w-4 items-center justify-center rounded-full bg-red-500 text-[9px] font-bold text-white shadow-sm ring-2 ring-[#0f1115]"
							>
								!
							</span>
						{/if}
					</a>
				{/each}
			</nav>

			<!-- Desktop CTA -->
			<div class="hidden items-center gap-4 lg:flex">
				<a
					href="/freeskinspath"
					class="transform rounded bg-yellow-500 px-5 py-2 text-sm font-bold text-black shadow-[0_0_15px_rgba(234,179,8,0.2)] transition-all hover:-translate-y-0.5 hover:bg-yellow-400 hover:shadow-[0_0_20px_rgba(234,179,8,0.4)] active:translate-y-0"
				>
					Free Skins Path
				</a>
			</div>

			<!-- Mobile Menu Button -->
			<button
				class="p-2 text-gray-400 hover:text-white lg:hidden"
				onclick={toggleMobileMenu}
				aria-label="Toggle Menu"
			>
				{#if isMobileMenuOpen}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="24"
						height="24"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"
						></line></svg
					>
				{:else}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="24"
						height="24"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"
						></line><line x1="3" y1="18" x2="21" y2="18"></line></svg
					>
				{/if}
			</button>
		</div>

		<!-- Mobile Menu Dropdown -->
		{#if isMobileMenuOpen}
			<div
				class="animate-in slide-in-from-top-2 absolute top-20 left-0 w-full border-b border-white/10 bg-[#15171e] shadow-2xl lg:hidden"
			>
				<div class="flex flex-col gap-2 p-4">
					{#each navItems as item}
						<a
							href={item.href}
							class="rounded-lg px-4 py-3 text-base font-medium {$page.url.pathname === item.href
								? 'bg-white/10 text-white'
								: 'text-gray-400 hover:bg-white/5 hover:text-white'}"
							onclick={closeMobileMenu}
						>
							<div class="flex items-center justify-between">
								{item.label}
								{#if item.badge}
									<span
										class="rounded bg-yellow-500/20 px-2 py-0.5 text-xs font-bold text-yellow-500 uppercase"
										>{item.badge}</span
									>
								{/if}
							</div>
						</a>
					{/each}
					<div class="my-2 h-px bg-white/10"></div>
					<a
						href="/freeskinspath"
						class="w-full rounded bg-yellow-600 py-3 text-center font-bold text-black hover:bg-yellow-500"
						>Free Skins Path</a
					>
				</div>
			</div>
		{/if}
	</header>

	<!-- Main Content -->
	<main class="relative z-0 flex-grow">
		<!-- Background Effects -->
		<div class="pointer-events-none absolute inset-0 z-[-1] overflow-hidden">
			<div
				class="absolute top-[-10%] left-[-10%] h-[40%] w-[40%] rounded-full bg-purple-900/10 blur-[100px]"
			></div>
			<div
				class="absolute right-[-10%] bottom-[-10%] h-[40%] w-[40%] rounded-full bg-yellow-900/10 blur-[100px]"
			></div>
			<div
				class="absolute inset-0 bg-[url('https://grainy-gradients.vercel.app/noise.svg')] opacity-[0.03]"
			></div>
		</div>

		{@render children()}
	</main>

	<!-- Footer -->
	<footer class="border-t border-white/5 bg-[#0b0c0f] pt-16 pb-8">
		<div class="container mx-auto max-w-6xl px-4">
			<div class="mb-12 grid grid-cols-1 gap-12 md:grid-cols-2 lg:grid-cols-4">
				<!-- Brand -->
				<div class="space-y-4">
					<div class="flex items-center gap-2">
						<img
							src="/logo.png"
							alt="CS2 Giveaways Logo"
							width="32"
							height="32"
							class="h-8 w-8 opacity-80"
						/>
						<span class="font-display text-lg font-bold text-white"
							>CS2<span class="text-gray-500">GIVEAWAYS</span></span
						>
					</div>
					<p class="text-sm leading-relaxed text-gray-500">
						The premier destination for CS2 skins, promo codes, and honest site reviews. We help you
						get more value from your favorite games.
					</p>
					<div class="flex gap-4 pt-2">
						<a
							href="https://x.com/cs2giveawayspro"
							class="flex h-8 w-8 items-center justify-center rounded bg-white/5 text-gray-400 transition-colors hover:bg-[#1DA1F2] hover:text-white"
							aria-label="Twitter"
						>
							<span class="sr-only">Twitter</span>
							<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 24 24"
								><path
									d="M23 3a10.9 10.9 0 0 1-3.14 1.53 4.48 4.48 0 0 0-7.86 3v1A10.66 10.66 0 0 1 3 4s-4 9 5 13a11.64 11.64 0 0 1-7 2c9 5 20 0 20-11.5a4.5 4.5 0 0 0-.08-.83A7.72 7.72 0 0 0 23 3z"
								></path></svg
							>
						</a>
					</div>
				</div>

				<!-- Earn Links -->
				<div>
					<h3 class="font-display mb-4 text-sm font-bold tracking-wider text-white uppercase">
						Earn
					</h3>
					<ul class="space-y-2 text-sm text-gray-500">
						<li>
							<a href="/promos" class="transition-colors hover:text-yellow-500">Promo Codes</a>
						</li>
						<li><a href="/cases" class="transition-colors hover:text-yellow-500">Free Cases</a></li>
						<li>
							<a href="/sites" class="transition-colors hover:text-yellow-500">Best CS2 Sites</a>
						</li>
					</ul>
				</div>

				<!-- Support Links -->
				<div>
					<h3 class="font-display mb-4 text-sm font-bold tracking-wider text-white uppercase">
						Support
					</h3>
					<ul class="space-y-2 text-sm text-gray-500">
						<li><a href="/faq" class="transition-colors hover:text-yellow-500">How it works</a></li>
						<li>
							<a href="/terms" class="transition-colors hover:text-yellow-500">Terms of Service</a>
						</li>
						<li>
							<a href="/privacy" class="transition-colors hover:text-yellow-500">Privacy Policy</a>
						</li>
						<li>
							<a href="/contact" class="transition-colors hover:text-yellow-500">Contact Us</a>
						</li>
						<li>
							<a href="/blog" class="transition-colors hover:text-yellow-500">Blog</a>
						</li>
					</ul>
				</div>

				<!-- Made By -->
				<div class="flex flex-col items-start space-y-2 lg:items-center">
					<h3 class="font-display text-sm font-bold tracking-wider text-white uppercase">
						Made by:
					</h3>
					<a
						href="https://www.digipulation.com"
						target="_blank"
						rel="noopener noreferrer"
						class="transition-opacity hover:opacity-80"
					>
						<img
							src="/digipulation.png"
							alt="Digipulation"
							width="225"
							height="45"
							class="h-auto max-w-[225px]"
						/>
					</a>
				</div>
			</div>

			<div
				class="flex flex-col items-center justify-between gap-4 border-t border-white/5 pt-8 text-xs text-gray-600 md:flex-row"
			>
				<p>
					&copy; {currentYear} CS2Giveaways. All rights reserved. Not affiliated with Valve Corp.
				</p>
			</div>
		</div>
	</footer>
</div>

<style>
	:global(body) {
		font-family: 'JetBrains Mono Variable', monospace;
		margin: 0;
	}

	.font-display {
		font-family: 'Chakra Petch', sans-serif;
		font-weight: 600;
	}

	:global(::-webkit-scrollbar) {
		width: 8px;
	}
	:global(::-webkit-scrollbar-track) {
		background: #0f1115;
	}
	:global(::-webkit-scrollbar-thumb) {
		background: #2d3748;
		border-radius: 4px;
	}
	:global(::-webkit-scrollbar-thumb:hover) {
		background: #4a5568;
	}
</style>
