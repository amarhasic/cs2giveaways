<script>
	import { page } from '$app/stores';
	import '../app.css'; // CRITICAL: This imports Tailwind styles.
	
	// Svelte 5 Props for Layout
	let { children } = $props();

	// State for mobile menu
	let isMobileMenuOpen = $state(false);
	
	// Navigation items tailored for CS2 Promo/Giveaway niche
	const navItems = [
		{ label: 'Home', href: '/' },
		{ label: 'Promo Codes', href: '/promos', badge: 'Hot' },
		{ label: 'Free Cases', href: '/cases' },
		{ label: 'Giveaways', href: '/giveaways' },
		{ label: 'Top Sites', href: '/sites' },
		{ label: 'Blog', href: '/blog' }
	];

	function toggleMobileMenu() {
		isMobileMenuOpen = !isMobileMenuOpen;
	}

	function closeMobileMenu() {
		isMobileMenuOpen = false;
	}

	// Dynamic copyright year
	let currentYear = new Date().getFullYear();
</script>

<svelte:head>
	<title>CS2 Giveaways - Daily Skins & Promo Codes</title>
	<link rel="icon" href="/favicon.png" />
	<!-- Preconnect for performance -->
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="app-container min-h-screen bg-[#0f1115] text-gray-300 font-sans selection:bg-yellow-500/30 selection:text-yellow-200 flex flex-col">
	
	<!-- Top Bar: Updated Ticker -->
	<div class="bg-[#1a1d24] border-b border-white/5 py-1 text-xs font-medium text-center text-gray-400 hidden md:block">
		<span class="mx-2"><span class="text-green-400">●</span> New Codes Added Daily</span>
		<span class="mx-2 text-gray-600">|</span>
		<!-- Rough calculation of daily value across major sites (Roll, Hellcase, Keydrop etc) -->
		<span class="mx-2"><span class="text-yellow-500">★</span> $25,000+ Daily Giveaways</span>
		<span class="mx-2 text-gray-600">|</span>
		<span class="mx-2">🎁 Best Daily Free Case: <button class="text-white hover:underline cursor-pointer bg-transparent border-0 p-0 inline">Claim Now</button></span>
	</div>

	<!-- Header -->
	<header class="sticky top-0 z-50 w-full border-b border-white/5 bg-[#0f1115]/80 backdrop-blur-md supports-[backdrop-filter]:bg-[#0f1115]/60">
		<div class="container mx-auto px-4 h-20 flex items-center justify-between">
			
			<!-- Logo Area: Bigger, Transparent, No Border -->
			<a href="/" class="flex items-center gap-3 group" onclick={closeMobileMenu}>
				<!-- Increased size by ~10% (w-10 -> w-11/12) and removed background/borders -->
				<div class="relative w-11 h-11 shrink-0 flex items-center justify-center">
					<img src="/logo.png" alt="CS2G" width="44" height="44" class="w-full h-full object-contain opacity-90 group-hover:opacity-100 transition-opacity" />
				</div>
				<div class="flex flex-col">
					<!-- Increased text size (text-xl -> text-2xl) -->
					<span class="font-display font-bold text-2xl text-white leading-none tracking-wide group-hover:text-yellow-400 transition-colors">
						CS2<span class="text-yellow-500">GIVEAWAYS</span>
					</span>
					<!-- Increased motto size slightly -->
					<span class="text-xs text-gray-500 uppercase tracking-widest font-semibold">Win Skins Daily</span>
				</div>
			</a>

			<!-- Desktop Nav -->
			<nav class="hidden lg:flex items-center gap-1">
				{#each navItems as item}
					<a 
						href={item.href} 
						class="relative px-4 py-2 text-sm font-medium rounded-md transition-all duration-200 
						{$page.url.pathname === item.href 
							? 'text-white bg-white/10' 
							: 'text-gray-400 hover:text-white hover:bg-white/5'}"
					>
						{item.label}
						{#if item.badge}
							<span class="absolute -top-1 -right-1 flex h-4 w-4 items-center justify-center rounded-full bg-red-500 text-[9px] font-bold text-white shadow-sm ring-2 ring-[#0f1115]">
								!
							</span>
						{/if}
					</a>
				{/each}
			</nav>

			<!-- Desktop Actions: Removed Sign In -->
			<div class="hidden lg:flex items-center gap-4">
				<button class="px-5 py-2 bg-yellow-500 hover:bg-yellow-400 text-black font-bold text-sm rounded shadow-[0_0_15px_rgba(234,179,8,0.2)] hover:shadow-[0_0_20px_rgba(234,179,8,0.4)] transition-all transform hover:-translate-y-0.5 active:translate-y-0">
					Start Earning
				</button>
			</div>

			<!-- Mobile Menu Button -->
			<button 
				class="lg:hidden p-2 text-gray-400 hover:text-white"
				onclick={toggleMobileMenu}
				aria-label="Toggle Menu"
			>
				{#if isMobileMenuOpen}
					<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
				{:else}
					<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"></line><line x1="3" y1="18" x2="21" y2="18"></line></svg>
				{/if}
			</button>
		</div>

		<!-- Mobile Menu Dropdown -->
		{#if isMobileMenuOpen}
			<div class="lg:hidden absolute top-20 left-0 w-full bg-[#15171e] border-b border-white/10 shadow-2xl animate-in slide-in-from-top-2">
				<div class="flex flex-col p-4 gap-2">
					{#each navItems as item}
						<a 
							href={item.href} 
							class="px-4 py-3 rounded-lg text-base font-medium {$page.url.pathname === item.href ? 'bg-white/10 text-white' : 'text-gray-400 hover:bg-white/5 hover:text-white'}"
							onclick={closeMobileMenu}
						>
							<div class="flex items-center justify-between">
								{item.label}
								{#if item.badge}
									<span class="text-xs bg-yellow-500/20 text-yellow-500 px-2 py-0.5 rounded uppercase font-bold">{item.badge}</span>
								{/if}
							</div>
						</a>
					{/each}
					<div class="h-px bg-white/10 my-2"></div>
					<button class="w-full py-3 bg-yellow-600 hover:bg-yellow-500 text-black font-bold rounded">Connect Steam</button>
				</div>
			</div>
		{/if}
	</header>

	<!-- Main Content Wrapper -->
	<main class="flex-grow relative z-0">
		<!-- Subtle Background Grid/Glow -->
		<div class="absolute inset-0 pointer-events-none z-[-1] overflow-hidden">
			<div class="absolute top-[-10%] left-[-10%] w-[40%] h-[40%] rounded-full bg-purple-900/10 blur-[100px]"></div>
			<div class="absolute bottom-[-10%] right-[-10%] w-[40%] h-[40%] rounded-full bg-yellow-900/10 blur-[100px]"></div>
			<div class="absolute inset-0 bg-[url('https://grainy-gradients.vercel.app/noise.svg')] opacity-[0.03]"></div>
		</div>

		<!-- Svelte 5 Children Render -->
		{@render children()}
	</main>

	<!-- Footer -->
	<footer class="bg-[#0b0c0f] border-t border-white/5 pt-16 pb-8">
		<div class="container mx-auto px-4">
			<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 mb-12">
				<!-- Brand Column -->
				<div class="space-y-4">
					<div class="flex items-center gap-2">
						<img src="/logo.png" alt="Logo" width="32" height="32" class="w-8 h-8 opacity-80" />
						<span class="font-display font-bold text-lg text-white">CS2<span class="text-gray-500">GIVEAWAYS</span></span>
					</div>
					<p class="text-gray-500 text-sm leading-relaxed">
						The premier destination for CS2 skins, promo codes, and honest site reviews. We help you get more value from your favorite games.
					</p>
					<div class="flex gap-4 pt-2">
						<!-- Social Placeholders -->
						<a href="/" class="w-8 h-8 rounded bg-white/5 flex items-center justify-center text-gray-400 hover:bg-[#5865F2] hover:text-white transition-colors" aria-label="Discord"><span class="sr-only">Discord</span><svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515.074.074 0 0 0-.079.037 2.382 2.382 0 0 0-.332.686 18.355 18.355 0 0 0-5.833 0 2.458 2.458 0 0 0-.336-.685.074.074 0 0 0-.08-.037 19.736 19.736 0 0 0-4.885 1.515.072.072 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057 19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028 14.09 14.09 0 0 0 1.226-1.994.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .085.028 19.92 19.92 0 0 0 5.996-3.03.077.077 0 0 0 .032-.057c.532-5.463-.829-9.156-3.996-13.626a.076.076 0 0 0-.032-.027zM8.02 15.33c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.956-2.419 2.157-2.419 1.21 0 2.176 1.096 2.157 2.42 0 1.333-.956 2.418-2.157 2.418zm7.975 0c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.955-2.419 2.157-2.419 1.21 0 2.176 1.096 2.157 2.42 0 1.333-.946 2.418-2.157 2.418z"/></svg></a>
						<a href="/" class="w-8 h-8 rounded bg-white/5 flex items-center justify-center text-gray-400 hover:bg-[#1DA1F2] hover:text-white transition-colors" aria-label="Twitter"><span class="sr-only">Twitter</span><svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M23 3a10.9 10.9 0 0 1-3.14 1.53 4.48 4.48 0 0 0-7.86 3v1A10.66 10.66 0 0 1 3 4s-4 9 5 13a11.64 11.64 0 0 1-7 2c9 5 20 0 20-11.5a4.5 4.5 0 0 0-.08-.83A7.72 7.72 0 0 0 23 3z"></path></svg></a>
					</div>
				</div>

				<!-- Links -->
				<div>
					<h3 class="text-white font-display font-bold mb-4 uppercase text-sm tracking-wider">Earn</h3>
					<ul class="space-y-2 text-sm text-gray-500">
						<li><a href="/promos" class="hover:text-yellow-500 transition-colors">Promo Codes</a></li>
						<li><a href="/cases" class="hover:text-yellow-500 transition-colors">Free Cases</a></li>
						<li><a href="/bonuses" class="hover:text-yellow-500 transition-colors">Deposit Bonuses</a></li>
						<li><a href="/referrals" class="hover:text-yellow-500 transition-colors">Referral Programs</a></li>
					</ul>
				</div>

				<div>
					<h3 class="text-white font-display font-bold mb-4 uppercase text-sm tracking-wider">Support</h3>
					<ul class="space-y-2 text-sm text-gray-500">
						<li><a href="/faq" class="hover:text-yellow-500 transition-colors">How it works</a></li>
						<li><a href="/terms" class="hover:text-yellow-500 transition-colors">Terms of Service</a></li>
						<li><a href="/privacy" class="hover:text-yellow-500 transition-colors">Privacy Policy</a></li>
						<li><a href="/contact" class="hover:text-yellow-500 transition-colors">Contact Us</a></li>
					</ul>
				</div>

				<!-- Removed 18+ Disclaimer Block -->
			</div>

			<div class="border-t border-white/5 pt-8 flex flex-col md:flex-row items-center justify-between gap-4 text-xs text-gray-600">
				<p>&copy; {currentYear} CS2Giveaways. All rights reserved. Not affiliated with Valve Corp.</p>
				<!-- Removed GamCare/BeGambleAware links -->
			</div>
		</div>
	</footer>
</div>

<style>
	/* Custom Fonts (assuming you might configure Tailwind config, but including base here) */
	:global(body) {
		font-family: 'Inter', sans-serif;
	}
	
	.font-display {
		font-family: 'Rajdhani', sans-serif;
	}

	/* Scrollbar Styling for a "gaming" feel */
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