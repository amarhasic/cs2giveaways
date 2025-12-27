<script>
	import { onMount } from 'svelte';

	// Promo codes from freeskinspath - same sites and affiliate codes
	const promoCodes = [
		{
			id: 1,
			site: 'CSGOEmpire',
			code: 'digipulation',
			reward: '1 Free Case',
			description: 'The OG CS2 gambling site. Sign up and claim your free case instantly!',
			url: 'https://csgoempire.com/r/digipulation',
			logo: '/csgoempire.jpg',
			tag: 'INSTANT'
		},
		{
			id: 2,
			site: 'Key-Drop',
			code: 'DIGIPULATION',
			reward: '1 Free Case',
			description: 'One of the biggest case opening sites. Get your free case with our code!',
			url: 'https://kd.link/?code=DIGIPULATION',
			logo: '/keydrop.jpg',
			tag: 'POPULAR'
		},
		{
			id: 3,
			site: 'Howl.gg',
			code: 'dpl',
			reward: '1 Free Case',
			description: 'Premium case battles and upgrades. Start with a free case on us!',
			url: 'https://howl.gg/r/dpl',
			logo: '/howl.jpg',
			tag: 'NEW'
		},
		{
			id: 4,
			site: 'Clash.gg',
			code: 'DIGIPULATION',
			reward: '3 Free Cases',
			description: 'The best value! Get 3 FREE cases just for signing up. Triple the chances!',
			url: 'https://clash.gg/r/DIGIPULATION',
			logo: '/clash.jpg',
			tag: 'BEST VALUE'
		},
		{
			id: 5,
			site: 'DatDrop',
			code: 'digipulation',
			reward: 'Daily Free Case',
			description: 'Get a FREE case every single day! Come back daily for more chances to win!',
			url: 'https://datdrop.com/p/digipulation',
			logo: '/datdrop.jpg',
			tag: 'DAILY'
		},
		{
			id: 6,
			site: '500 Casino',
			code: 'DIGIPULATION',
			reward: '100% Deposit Bonus + 50 Free Spins',
			description: 'Claim 100% Deposit Bonus up to $1k and 50 Free Spins on signup!',
			url: 'https://500.casino/r/DIGIPULATION',
			logo: '/500casino.jpg',
			tag: 'HOT BONUS'
		},
		{
			id: 7,
			site: 'Roobet',
			code: 'digipulation',
			reward: 'Rakeback & Rewards',
			description: 'Get daily, weekly, and monthly rewards plus rakeback on all your plays!',
			url: 'https://roobet.com/?ref=digipulation',
			logo: '/roobet.jpg',
			tag: 'RAKEBACK'
		},
		{
			id: 8,
			site: 'Gamdom',
			code: 'digipulation',
			reward: '15% Rakeback + Up to 60%',
			description:
				'Claim 15% instant rakeback for the first 7 days upon Signup + Earn up to 60% Rakeback!',
			url: 'https://gamdom.com/r/digipulation',
			logo: '/gamdom.jpg',
			tag: 'TOP RAKEBACK'
		},
		{
			id: 9,
			site: 'Farmskins',
			code: 'digipulation',
			reward: '$1 Free + Daily Cases',
			description: 'Get free $1 balance to get skins plus daily free cases on Farmskins!',
			url: 'https://farmskins.com/ref-digipulation',
			logo: '/farmskins.jpg',
			tag: 'FREE SKINS'
		},
		{
			id: 10,
			site: 'Casehug',
			code: 'DIGI',
			reward: '$1 Free + 20% Bonus',
			description: 'Get $1 free bonus on signup and 20% extra on every top up!',
			url: 'https://casehug.com/r/DIGI',
			logo: '/casehug.jpg',
			tag: 'BONUS'
		},
		{
			id: 11,
			site: 'CSGOLuck',
			code: 'DIGI',
			reward: 'Special Rewards',
			description: 'Visit CSGOLuck and claim your exclusive rewards with our promo code!',
			url: 'https://csgoluck.com/rewards',
			logo: '/csgoluck.jpg',
			tag: 'REWARDS'
		}
	];

	// State for each code
	let codesState = $state(
		promoCodes.map((code) => ({
			...code,
			isGenerating: false,
			isCopied: false,
			copyCountdown: 0,
			currentCode: '',
			isAnimating: false
		}))
	);

	// Generate random characters for animation
	function generateRandomChars(length) {
		const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
		let result = '';
		for (let i = 0; i < length; i++) {
			result += chars.charAt(Math.floor(Math.random() * chars.length));
		}
		return result;
	}

	// Generate a promo code with animation (0.8 seconds)
	function generateCode(index) {
		const codeState = codesState[index];
		if (codeState.isGenerating || codeState.isCopied) return;

		codeState.isGenerating = true;
		codeState.isAnimating = true;
		codeState.currentCode = generateRandomChars(codeState.code.length);

		let iterations = 0;
		const maxIterations = 10; // 0.8 seconds / 80ms = 10 iterations
		const interval = setInterval(() => {
			if (iterations >= maxIterations) {
				clearInterval(interval);
				codeState.currentCode = codeState.code;
				codeState.isAnimating = false;
				codeState.isGenerating = false;
				return;
			}

			codeState.currentCode = generateRandomChars(codeState.code.length);
			iterations++;
		}, 80);
	}

	// Copy code to clipboard
	async function copyCode(index) {
		const codeState = codesState[index];
		if (codeState.isCopied) return;

		try {
			await navigator.clipboard.writeText(codeState.code);
			codeState.isCopied = true;
			codeState.copyCountdown = 300; // 5 minutes in seconds

			// Start countdown
			const countdownInterval = setInterval(() => {
				codeState.copyCountdown--;
				if (codeState.copyCountdown <= 0) {
					clearInterval(countdownInterval);
					codeState.isCopied = false;
				}
			}, 1000);
		} catch (err) {
			console.error('Failed to copy code: ', err);
		}
	}

	// Format countdown time
	function formatCountdown(seconds) {
		const mins = Math.floor(seconds / 60);
		const secs = seconds % 60;
		return `${mins}:${secs.toString().padStart(2, '0')}`;
	}

	// SEO Data
	const schemaOrg = {
		'@context': 'https://schema.org',
		'@type': 'ItemList',
		itemListElement: promoCodes.map((promo, index) => ({
			'@type': 'ListItem',
			position: index + 1,
			url: `https://cs2giveaways.com/promos#${promo.site.toLowerCase().replace(/\s+/g, '-')}`,
			name: `${promo.site} Promo Code: ${promo.code}`,
			description: promo.description
		}))
	};
</script>

<svelte:head>
	<title>CS2 Promo Codes 2025 | {promoCodes.length}+ Verified Codes for Free Cases</title>
	<meta
		name="description"
		content="Verified CS2 promo codes for Empire, Key-Drop, DatDrop, & more. Claim {promoCodes.length}+ free cases and deposit bonuses instantly. Updated daily for 2025."
	/>
	<meta
		name="keywords"
		content="CS2 promo codes, CSGO affiliate codes, free CS2 cases, CS2 gambling codes, {promoCodes
			.map((p) => p.site)
			.join(', ')}"
	/>

	<!-- JSON-LD Structured Data -->
	{@html `<script type="application/ld+json">${JSON.stringify(schemaOrg)}</script>`}
</svelte:head>

<div
	class="min-h-screen"
	style="background: linear-gradient(135deg, #28397F 0%, #1a2550 50%, #0f1629 100%);"
>
	<!-- Hero Section -->
	<section class="relative overflow-hidden py-16 md:py-24">
		<!-- Animated background with brand colors -->
		<div class="absolute inset-0 overflow-hidden">
			<div
				class="absolute top-1/4 left-1/4 h-96 w-96 animate-pulse rounded-full blur-[120px]"
				style="background: rgba(251, 172, 24, 0.4);"
			></div>
			<div
				class="absolute right-1/4 bottom-1/4 h-96 w-96 animate-pulse rounded-full blur-[120px]"
				style="background: rgba(40, 57, 127, 0.6); animation-delay: 1s;"
			></div>
			<div
				class="absolute top-1/2 left-1/2 h-[600px] w-[600px] -translate-x-1/2 -translate-y-1/2 rounded-full blur-[150px]"
				style="background: rgba(251, 172, 24, 0.15);"
			></div>
		</div>

		<div class="relative z-10 container mx-auto max-w-6xl px-4">
			<div class="text-center">
				<!-- Badge -->
				<div
					class="mb-6 inline-flex items-center gap-2 rounded-full px-5 py-2.5"
					style="background: rgba(251, 172, 24, 0.25); border: 2px solid #FBAC18;"
				>
					<span class="relative flex h-3 w-3">
						<span
							class="absolute inline-flex h-full w-full animate-ping rounded-full opacity-75"
							style="background: #FBAC18;"
						></span>
						<span class="relative inline-flex h-3 w-3 rounded-full" style="background: #FBAC18;"
						></span>
					</span>
					<span class="text-sm font-bold" style="color: #FBAC18;"
						>Exclusive Codes • Updated Daily</span
					>
				</div>

				<!-- Main heading -->
				<h1 class="mb-6 text-4xl font-bold md:text-6xl lg:text-7xl">
					<span style="color: #FBAC18;">Promo</span>
					<span class="text-white"> Codes</span>
				</h1>

				<p
					class="mx-auto mb-8 max-w-2xl text-lg md:text-xl"
					style="color: rgba(255, 255, 255, 0.8);"
				>
					Click <span class="font-bold" style="color: #FBAC18;">"GENERATE KEY"</span> to reveal your
					unique code. Each code feels freshly generated just for you!
				</p>

				<!-- Stats -->
				<div class="flex flex-wrap justify-center gap-4 md:gap-8">
					<div
						class="rounded-xl px-6 py-3"
						style="background: rgba(251, 172, 24, 0.2); border: 2px solid #FBAC18;"
					>
						<span class="text-2xl font-bold" style="color: #FBAC18;">100%</span>
						<span class="ml-2 text-white">Free Codes</span>
					</div>
					<div
						class="rounded-xl px-6 py-3"
						style="background: rgba(251, 172, 24, 0.2); border: 2px solid #FBAC18;"
					>
						<span class="text-2xl font-bold" style="color: #FBAC18;">7+</span>
						<span class="ml-2 text-white">Free Cases</span>
					</div>
					<div
						class="rounded-xl px-6 py-3"
						style="background: rgba(251, 172, 24, 0.2); border: 2px solid #FBAC18;"
					>
						<span class="text-2xl font-bold" style="color: #FBAC18;">$100s</span>
						<span class="ml-2 text-white">Potential Wins</span>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Promo Cards Section -->
	<section class="py-12 pb-16">
		<div class="container mx-auto max-w-6xl px-4">
			<div class="grid gap-8 md:grid-cols-2 lg:grid-cols-3">
				{#each codesState as codeState, index}
					<div class="group relative">
						<!-- Glow effect on hover -->
						<div
							class="absolute -inset-2 rounded-3xl opacity-0 blur-xl transition-opacity duration-500 group-hover:opacity-50"
							style="background: linear-gradient(90deg, #FBAC18, #28397F, #FBAC18);"
						></div>

						<!-- Card -->
						<div
							class="relative rounded-2xl p-6 transition-all duration-300 hover:border-2"
							style="background: linear-gradient(135deg, rgba(40, 57, 127, 0.5) 0%, rgba(26, 29, 36, 0.9) 100%); border: 2px solid #FBAC18;"
						>
							<!-- Tag -->
							<div class="absolute -top-3 right-4">
								<span
									class="rounded-full px-4 py-1.5 text-xs font-black"
									style="background: #FBAC18; color: #28397F; box-shadow: 0 4px 15px rgba(251, 172, 24, 0.4);"
								>
									{codeState.tag}
								</span>
							</div>

							<!-- Logo & Site -->
							<div class="mb-5 flex items-center gap-4">
								<div
									class="flex h-14 w-14 items-center justify-center overflow-hidden rounded-xl"
									style="background: rgba(251, 172, 24, 0.25); border: 2px solid #FBAC18;"
								>
									<img
										src={codeState.logo}
										alt="{codeState.site} logo"
										class="h-full w-full object-cover"
									/>
								</div>
								<div>
									<h3 class="text-xl font-bold text-white">{codeState.site}</h3>
									<span class="text-sm font-bold" style="color: #FBAC18;">{codeState.reward}</span>
								</div>
							</div>

							<!-- Description -->
							<p class="mb-6 text-sm" style="color: rgba(255, 255, 255, 0.7);">
								{codeState.description}
							</p>

							<!-- Code Display Box -->
							<div class="mb-5">
								<div
									class="flex min-h-[70px] items-center justify-center rounded-xl p-5 text-center font-mono text-2xl tracking-widest"
									style="background: rgba(40, 57, 127, 0.4); border: 2px solid #FBAC18;"
								>
									{#if codeState.isAnimating}
										<span
											class="animate-pulse font-black"
											style="color: #FBAC18; text-shadow: 0 0 10px rgba(251, 172, 24, 0.5);"
											>{codeState.currentCode}</span
										>
									{:else if codeState.currentCode}
										<span
											class="font-black"
											style="color: #FBAC18; text-shadow: 0 0 10px rgba(251, 172, 24, 0.5);"
											>{codeState.currentCode}</span
										>
									{:else}
										<span style="color: rgba(251, 172, 24, 0.4);">••••••••••</span>
									{/if}
								</div>
							</div>

							<!-- Action Button -->
							{#if codeState.isCopied}
								<button
									class="flex w-full items-center justify-center gap-3 rounded-xl px-4 py-4 text-base font-black"
									style="background: #dc2626; border: 2px solid #ef4444; color: white; cursor: not-allowed;"
									disabled
								>
									<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
										></path>
									</svg>
									<span>COPIED - USE WITHIN {formatCountdown(codeState.copyCountdown)}</span>
								</button>
							{:else if codeState.currentCode && !codeState.isGenerating}
								<button
									class="flex w-full cursor-pointer items-center justify-center gap-3 rounded-xl px-4 py-4 text-base font-black transition-all duration-300 hover:scale-105"
									style="background: #FBAC18; border: 2px solid #FBAC18; color: #28397F; box-shadow: 0 4px 20px rgba(251, 172, 24, 0.4);"
									onclick={() => copyCode(index)}
								>
									<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"
										></path>
									</svg>
									<span>COPY CODE</span>
								</button>
							{:else}
								<button
									class="flex w-full cursor-pointer items-center justify-center gap-3 rounded-xl px-4 py-4 text-base font-black transition-all duration-300 hover:scale-105"
									style="background: #28397F; border: 2px solid #FBAC18; color: #FBAC18; box-shadow: 0 4px 20px rgba(40, 57, 127, 0.4);"
									onclick={() => generateCode(index)}
									disabled={codeState.isGenerating}
								>
									{#if codeState.isGenerating}
										<svg
											class="h-6 w-6 animate-spin"
											xmlns="http://www.w3.org/2000/svg"
											fill="none"
											viewBox="0 0 24 24"
										>
											<circle
												class="opacity-25"
												cx="12"
												cy="12"
												r="10"
												stroke="currentColor"
												stroke-width="4"
											></circle>
											<path
												class="opacity-75"
												fill="currentColor"
												d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
											></path>
										</svg>
										<span>GENERATING...</span>
									{:else}
										<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M15 7a2 2 0 012 2m4 0a6 6 0 01-7.743 5.743L11 17H9v2H7v2H4a1 1 0 01-1-1v-2.586a1 1 0 01.293-.707l5.964-5.964A6 6 0 1121 9z"
											></path>
										</svg>
										<span>GENERATE KEY</span>
									{/if}
								</button>
							{/if}

							<!-- Visit Site Link -->
							{#if codeState.currentCode && !codeState.isGenerating}
								<a
									href={codeState.url}
									target="_blank"
									rel="noopener noreferrer"
									class="mt-4 block w-full cursor-pointer rounded-xl px-4 py-3 text-center text-sm font-bold transition-all duration-300 hover:scale-105"
									style="border: 2px solid #FBAC18; color: #FBAC18; background: transparent;"
									onmouseenter={(e) => {
										e.target.style.background = '#FBAC18';
										e.target.style.color = '#28397F';
									}}
									onmouseleave={(e) => {
										e.target.style.background = 'transparent';
										e.target.style.color = '#FBAC18';
									}}
								>
									Visit {codeState.site} →
								</a>
							{/if}
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<!-- Info Banner -->
	<section class="py-8">
		<div class="container mx-auto max-w-6xl px-4">
			<div class="rounded-2xl p-6 md:p-8" style="background: #28397F; border: 3px solid #FBAC18;">
				<div class="flex flex-col items-center justify-between gap-6 md:flex-row">
					<div class="flex items-center gap-4">
						<div
							class="flex h-16 w-16 items-center justify-center rounded-xl"
							style="background: #FBAC18;"
						>
							<svg
								class="h-8 w-8"
								style="color: #28397F;"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
								></path>
							</svg>
						</div>
						<div>
							<h3 class="text-xl font-black" style="color: #FBAC18;">Unique Code Generation</h3>
							<p class="text-sm" style="color: rgba(255, 255, 255, 0.8);">
								Each code is generated uniquely for you • Use within 5 minutes for best results
							</p>
						</div>
					</div>
					<a
						href="/freeskinspath"
						class="cursor-pointer rounded-xl px-8 py-4 font-black whitespace-nowrap transition-all duration-300 hover:scale-105"
						style="background: #FBAC18; color: #28397F; box-shadow: 0 4px 20px rgba(251, 172, 24, 0.4);"
					>
						Get Free Cases →
					</a>
				</div>
			</div>
		</div>
	</section>

	<!-- How It Works Section -->
	<section class="py-16">
		<div class="container mx-auto max-w-6xl px-4">
			<div class="mb-12 text-center">
				<h2 class="mb-4 text-3xl font-bold md:text-4xl">
					<span style="color: #FBAC18;">How to</span>
					<span class="text-white"> Use Promo Codes</span>
				</h2>
			</div>

			<div class="grid gap-8 md:grid-cols-3">
				<div
					class="rounded-2xl p-8 text-center transition-all duration-300 hover:scale-105"
					style="background: rgba(40, 57, 127, 0.5); border: 2px solid #FBAC18;"
				>
					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl"
						style="background: #FBAC18;"
					>
						<span class="text-3xl font-black" style="color: #28397F;">1</span>
					</div>
					<h3 class="mb-3 text-xl font-bold" style="color: #FBAC18;">Generate Your Key</h3>
					<p style="color: rgba(255, 255, 255, 0.8);">
						Click "GENERATE KEY" to reveal your unique promo code with a cool animation
					</p>
				</div>

				<div
					class="rounded-2xl p-8 text-center transition-all duration-300 hover:scale-105"
					style="background: rgba(40, 57, 127, 0.5); border: 2px solid #FBAC18;"
				>
					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl"
						style="background: #FBAC18;"
					>
						<span class="text-3xl font-black" style="color: #28397F;">2</span>
					</div>
					<h3 class="mb-3 text-xl font-bold" style="color: #FBAC18;">Copy the Code</h3>
					<p style="color: rgba(255, 255, 255, 0.8);">
						Click "COPY CODE" to copy it to your clipboard - use within 5 minutes!
					</p>
				</div>

				<div
					class="rounded-2xl p-8 text-center transition-all duration-300 hover:scale-105"
					style="background: rgba(40, 57, 127, 0.5); border: 2px solid #FBAC18;"
				>
					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl"
						style="background: #FBAC18;"
					>
						<span class="text-3xl font-black" style="color: #28397F;">3</span>
					</div>
					<h3 class="mb-3 text-xl font-bold" style="color: #FBAC18;">Claim Your Bonus</h3>
					<p style="color: rgba(255, 255, 255, 0.8);">
						Visit the site and enter your code to claim your free cases and bonuses
					</p>
				</div>
			</div>
		</div>
	</section>

	<!-- CTA Section -->
	<section class="py-16">
		<div class="container mx-auto max-w-4xl px-4 text-center">
			<div
				class="rounded-3xl p-8 md:p-12"
				style="background: #28397F; border: 4px solid #FBAC18; box-shadow: 0 0 40px rgba(251, 172, 24, 0.3);"
			>
				<h2 class="mb-4 text-3xl font-bold text-white md:text-4xl">
					Want Even More <span style="color: #FBAC18;">Free Skins?</span>
				</h2>
				<p class="mx-auto mb-8 max-w-xl" style="color: rgba(255, 255, 255, 0.8);">
					Follow our complete Free Skins Path to claim 7+ free cases from all our partner sites!
				</p>
				<a
					href="/freeskinspath"
					class="inline-flex transform cursor-pointer items-center gap-3 rounded-full px-10 py-5 text-lg font-black transition-all duration-300 hover:scale-105"
					style="background: #FBAC18; color: #28397F; box-shadow: 0 4px 30px rgba(251, 172, 24, 0.5);"
				>
					<span>Start Free Skins Path</span>
					<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M13 7l5 5m0 0l-5 5m5-5H6"
						></path>
					</svg>
				</a>
			</div>
		</div>
	</section>
</div>

<style>
	.font-display {
		font-family: 'Chakra Petch', sans-serif;
	}
</style>
