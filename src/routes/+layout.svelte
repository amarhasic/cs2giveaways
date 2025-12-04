<script>
	import { page } from '$app/stores';
	import '../app.css';
	
	let { children } = $props();
	let isMobileMenuOpen = $state(false);
	
	const navItems = [
		{ label: 'Home', href: '/' },
		{ label: 'Promo Codes', href: '/promos', badge: 'Hot' },
		{ label: 'Free Cases', href: '/cases' },
		{ label: 'Giveaways', href: '/giveaways' },
		{ label: 'Top Sites', href: '/sites' }
	];

	function toggleMobileMenu() {
		isMobileMenuOpen = !isMobileMenuOpen;
	}

	function closeMobileMenu() {
		isMobileMenuOpen = false;
	}

	let currentYear = new Date().getFullYear();
</script>

<svelte:head>
	<title>CS2 Giveaways - Daily Skins & Promo Codes</title>
	<link rel="icon" href="/favicon.png" />
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="app-container min-h-screen bg-[#0f1115] text-gray-300 font-sans selection:bg-yellow-500/30 selection:text-yellow-200 flex flex-col">
	
	<!-- Top Bar -->
	<div class="bg-[#1a1d24] border-b border-white/5 py-1 text-xs font-medium text-center text-gray-400 hidden md:block">
		<span class="mx-2"><span class="text-green-400">●</span> New Codes Added Daily</span>
		<span class="mx-2 text-gray-600">|</span>
		<span class="mx-2"><span class="text-yellow-500">★</span> Up-to-date Daily Giveaways</span>
	</div>

	<!-- Header -->
	<header class="sticky top-0 z-50 w-full border-b border-white/5 bg-[#0f1115]/80 backdrop-blur-md supports-[backdrop-filter]:bg-[#0f1115]/60">
		<div class="container mx-auto max-w-6xl px-4 h-20 flex items-center justify-between">
			
			<!-- Logo -->
			<a href="/" class="flex items-center gap-3 group" onclick={closeMobileMenu}>
				<div class="relative w-11 h-11 shrink-0 flex items-center justify-center">
					<img src="/logo.png" alt="CS2G" width="44" height="44" class="w-full h-full object-contain opacity-90 group-hover:opacity-100 transition-opacity" />
				</div>
				<div class="flex flex-col">
					<span class="font-display font-bold text-2xl text-white leading-none tracking-wide group-hover:text-yellow-400 transition-colors">
						CS2<span class="text-yellow-500">GIVEAWAYS</span>
					</span>
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

			<!-- Desktop CTA -->
			<div class="hidden lg:flex items-center gap-4">
				<a href="/freeskinspath" class="px-5 py-2 bg-yellow-500 hover:bg-yellow-400 text-black font-bold text-sm rounded shadow-[0_0_15px_rgba(234,179,8,0.2)] hover:shadow-[0_0_20px_rgba(234,179,8,0.4)] transition-all transform hover:-translate-y-0.5 active:translate-y-0">
					Free Skins Path
				</a>
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
					<a href="/freeskinspath" class="w-full py-3 bg-yellow-600 hover:bg-yellow-500 text-black font-bold rounded text-center">Free Skins Path</a>
				</div>
			</div>
		{/if}
	</header>

	<!-- Main Content -->
	<main class="flex-grow relative z-0">
		<!-- Background Effects -->
		<div class="absolute inset-0 pointer-events-none z-[-1] overflow-hidden">
			<div class="absolute top-[-10%] left-[-10%] w-[40%] h-[40%] rounded-full bg-purple-900/10 blur-[100px]"></div>
			<div class="absolute bottom-[-10%] right-[-10%] w-[40%] h-[40%] rounded-full bg-yellow-900/10 blur-[100px]"></div>
			<div class="absolute inset-0 bg-[url('https://grainy-gradients.vercel.app/noise.svg')] opacity-[0.03]"></div>
		</div>

		{@render children()}
	</main>

	<!-- Footer -->
	<footer class="bg-[#0b0c0f] border-t border-white/5 pt-16 pb-8">
		<div class="container mx-auto max-w-6xl px-4">
			<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-12 mb-12">
				
				<!-- Brand -->
				<div class="space-y-4">
					<div class="flex items-center gap-2">
						<img src="/logo.png" alt="Logo" width="32" height="32" class="w-8 h-8 opacity-80" />
						<span class="font-display font-bold text-lg text-white">CS2<span class="text-gray-500">GIVEAWAYS</span></span>
					</div>
					<p class="text-gray-500 text-sm leading-relaxed">
						The premier destination for CS2 skins, promo codes, and honest site reviews. We help you get more value from your favorite games.
					</p>
					<div class="flex gap-4 pt-2">
						<a href="/" class="w-8 h-8 rounded bg-white/5 flex items-center justify-center text-gray-400 hover:bg-[#1DA1F2] hover:text-white transition-colors" aria-label="Twitter">
							<span class="sr-only">Twitter</span>
							<svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M23 3a10.9 10.9 0 0 1-3.14 1.53 4.48 4.48 0 0 0-7.86 3v1A10.66 10.66 0 0 1 3 4s-4 9 5 13a11.64 11.64 0 0 1-7 2c9 5 20 0 20-11.5a4.5 4.5 0 0 0-.08-.83A7.72 7.72 0 0 0 23 3z"></path></svg>
						</a>
					</div>
				</div>

				<!-- Earn Links -->
				<div>
					<h3 class="text-white font-display font-bold mb-4 uppercase text-sm tracking-wider">Earn</h3>
					<ul class="space-y-2 text-sm text-gray-500">
						<li><a href="/promos" class="hover:text-yellow-500 transition-colors">Promo Codes</a></li>
						<li><a href="/cases" class="hover:text-yellow-500 transition-colors">Free Cases</a></li>
						<li><a href="/bonuses" class="hover:text-yellow-500 transition-colors">Deposit Bonuses</a></li>
						<li><a href="/referrals" class="hover:text-yellow-500 transition-colors">Referral Programs</a></li>
					</ul>
				</div>

				<!-- Support Links -->
				<div>
					<h3 class="text-white font-display font-bold mb-4 uppercase text-sm tracking-wider">Support</h3>
					<ul class="space-y-2 text-sm text-gray-500">
						<li><a href="/faq" class="hover:text-yellow-500 transition-colors">How it works</a></li>
						<li><a href="/terms" class="hover:text-yellow-500 transition-colors">Terms of Service</a></li>
						<li><a href="/privacy" class="hover:text-yellow-500 transition-colors">Privacy Policy</a></li>
						<li><a href="/contact" class="hover:text-yellow-500 transition-colors">Contact Us</a></li>
					</ul>
				</div>

				<!-- Made By -->
				<div class="flex flex-col items-start lg:items-center space-y-2">
					<h3 class="text-white font-display font-bold uppercase text-sm tracking-wider">Made by:</h3>
					<a href="https://www.digipulation.com" target="_blank" rel="noopener noreferrer" class="transition-opacity hover:opacity-80">
						<img src="/digipulation.png" alt="Digipulation" width="225" height="45" class="h-auto max-w-[225px]" />
					</a>
				</div>
			</div>

			<div class="border-t border-white/5 pt-8 flex flex-col md:flex-row items-center justify-between gap-4 text-xs text-gray-600">
				<p>&copy; {currentYear} CS2Giveaways. All rights reserved. Not affiliated with Valve Corp.</p>
			</div>
		</div>
	</footer>
</div>

<style>
	:global(body) {
		font-family: 'Inter', sans-serif;
	}
	
	.font-display {
		font-family: 'Rajdhani', sans-serif;
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