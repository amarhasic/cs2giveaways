<script>
	let selectedFilter = $state('all');
	let copiedCode = $state(null);

	const promoCodes = [
		{
			id: 1,
			site: 'CSGOEmpire',
			logo: '/csgoempire-affiliate.png',
			code: 'catweazle',
			bonus: 'Free $0.50 + 5% Deposit Bonus',
			category: ['free-coins', 'deposit-bonus'],
			claimUrl: 'https://csgoempire.com/r/catweazle',
			verified: '2025-12-05',
			featured: true
		},
		{
			id: 2,
			site: 'Key-Drop',
			logo: '/keydrop-promo-code.png',
			code: 'CATWZ',
			bonus: 'Free $0.50 + 5% Bonus',
			category: ['free-coins', 'deposit-bonus'],
			claimUrl: 'https://kd.link/?code=CATWZ',
			verified: '2025-12-05',
			featured: true
		},
		{
			id: 3,
			site: 'Clash.gg',
			logo: '/clashgg-affiliate-offer.jpg',
			code: 'CATW',
			bonus: 'Free Cases + Deposit Bonus',
			category: ['free-cases', 'deposit-bonus'],
			claimUrl: 'https://clash.gg/r/CATW',
			verified: '2025-12-05',
			featured: true
		}
	];

	const filters = [
		{ id: 'all', label: 'All Codes', count: promoCodes.length },
		{ id: 'free-coins', label: 'Free Coins', count: promoCodes.filter(p => p.category.includes('free-coins')).length },
		{ id: 'free-cases', label: 'Free Cases', count: promoCodes.filter(p => p.category.includes('free-cases')).length },
		{ id: 'deposit-bonus', label: 'Deposit Bonus', count: promoCodes.filter(p => p.category.includes('deposit-bonus')).length }
	];

	function getFilteredCodes() {
		if (selectedFilter === 'all') return promoCodes;
		return promoCodes.filter(code => code.category.includes(selectedFilter));
	}

	function copyToClipboard(code) {
		navigator.clipboard.writeText(code).then(() => {
			copiedCode = code;
			setTimeout(() => {
				copiedCode = null;
			}, 2000);
		});
	}

	function formatDate(dateString) {
		const date = new Date(dateString);
		const today = new Date();
		const diffTime = Math.abs(today - date);
		const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
		
		if (diffDays === 0) return 'Today';
		if (diffDays === 1) return 'Yesterday';
		return `${diffDays} days ago`;
	}
</script>

<svelte:head>
	<title>CS2 Promo Codes - Free Skins & Bonuses | CS2 Giveaways</title>
	<meta name="description" content="Get exclusive CS2 promo codes for free skins, coins, and deposit bonuses. Updated daily with verified codes from CSGOEmpire, Key-Drop, Clash.gg and more." />
</svelte:head>

<div class="container mx-auto max-w-6xl px-4 py-12 space-y-12">
	
	<!-- Hero Section -->
	<section class="text-center space-y-6">
		<div class="flex justify-center mb-4">
			<svg viewBox="0 0 450 70" class="w-full max-w-lg h-auto" xmlns="http://www.w3.org/2000/svg">
				<defs>
					<linearGradient id="goldGradient" x1="0%" y1="0%" x2="100%" y2="0%">
						<stop offset="0%" style="stop-color:#f59e0b;stop-opacity:1" />
						<stop offset="50%" style="stop-color:#eab308;stop-opacity:1" />
						<stop offset="100%" style="stop-color:#f59e0b;stop-opacity:1" />
					</linearGradient>
					<filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
						<feGaussianBlur stdDeviation="3" result="coloredBlur"/>
						<feMerge>
							<feMergeNode in="coloredBlur"/>
							<feMergeNode in="SourceGraphic"/>
						</feMerge>
					</filter>
				</defs>
				
				<line x1="0" y1="35" x2="70" y2="35" stroke="url(#goldGradient)" stroke-width="2" opacity="0.5"/>
				<line x1="380" y1="35" x2="450" y2="35" stroke="url(#goldGradient)" stroke-width="2" opacity="0.5"/>
				
				<text x="80" y="44" fill="#eab308" font-size="24" filter="url(#glow)">★</text>
				<text x="360" y="44" fill="#eab308" font-size="24" filter="url(#glow)">★</text>
				
				<text x="225" y="32" text-anchor="middle" fill="url(#goldGradient)" font-family="Rajdhani, sans-serif" font-weight="700" font-size="28" letter-spacing="2" filter="url(#glow)">
					PROMO CODES
				</text>
				<text x="225" y="58" text-anchor="middle" fill="#ffffff" font-family="Rajdhani, sans-serif" font-weight="600" font-size="18" letter-spacing="5" opacity="0.9">
					VERIFIED DAILY
				</text>
			</svg>
		</div>

		<div class="max-w-2xl mx-auto space-y-4">
			<h1 class="text-4xl md:text-5xl font-display font-bold text-white">
				Exclusive CS2 <span class="text-yellow-500">Promo Codes</span>
			</h1>
			<p class="text-lg text-gray-400 leading-relaxed">
				Get free skins, coins, and deposit bonuses with our exclusive promo codes. All codes are verified daily and work on the most trusted CS2 skin sites.
			</p>
			<div class="flex items-center justify-center gap-2 text-sm">
				<span class="flex items-center gap-2 text-green-400">
					<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
					{promoCodes.length} Active Codes
				</span>
				<span class="text-gray-600">|</span>
				<span class="text-gray-400">Updated Today</span>
			</div>
		</div>
	</section>

	<!-- Filter Bar -->
	<section>
		<div class="flex flex-wrap justify-center gap-3">
			{#each filters as filter}
				<button
					onclick={() => selectedFilter = filter.id}
					class="px-6 py-3 rounded-lg font-medium text-sm transition-all duration-200 {selectedFilter === filter.id 
						? 'bg-yellow-500 text-black shadow-[0_0_20px_rgba(234,179,8,0.3)]' 
						: 'bg-white/5 text-gray-400 hover:bg-white/10 hover:text-white border border-white/10'}"
				>
					{filter.label}
					<span class="ml-2 {selectedFilter === filter.id ? 'text-black/70' : 'text-gray-600'}">{filter.count}</span>
				</button>
			{/each}
		</div>
	</section>

	<!-- Promo Code Cards -->
	<section>
		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
			{#each getFilteredCodes() as promo (promo.id)}
				<article class="group relative bg-[#1a1d24] border border-white/10 rounded-xl overflow-hidden hover:border-yellow-500/50 transition-all duration-300">
					
					<!-- Featured Badge -->
					{#if promo.featured}
						<div class="absolute top-3 right-3 z-10 flex items-center gap-1 px-2 py-1 bg-yellow-500 text-black text-xs font-bold uppercase rounded">
							<svg class="w-3 h-3" fill="currentColor" viewBox="0 0 20 20"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"></path></svg>
							Hot
						</div>
					{/if}

					<!-- Site Logo -->
					<div class="relative h-48 bg-gradient-to-br from-gray-900 to-black overflow-hidden">
						<img 
							src={promo.logo} 
							alt={promo.site}
							class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
						/>
						<div class="absolute inset-0 bg-gradient-to-t from-[#1a1d24] via-transparent to-transparent"></div>
					</div>

					<!-- Card Content -->
					<div class="p-5 space-y-4">
						
						<!-- Site Name & Verification -->
						<div class="flex items-start justify-between">
							<h3 class="text-xl font-display font-bold text-white">{promo.site}</h3>
							<span class="flex items-center gap-1 text-xs text-green-400">
								<svg class="w-3 h-3" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path></svg>
								{formatDate(promo.verified)}
							</span>
						</div>

						<!-- Bonus Description -->
						<p class="text-gray-400 text-sm leading-relaxed">{promo.bonus}</p>

						<!-- Promo Code -->
						<div class="space-y-2">
							<label class="text-xs text-gray-500 uppercase tracking-wider font-medium">Promo Code</label>
							<div class="flex gap-2">
								<button
									onclick={() => copyToClipboard(promo.code)}
									class="flex-1 px-4 py-3 bg-white/5 border-2 {copiedCode === promo.code ? 'border-green-500' : 'border-white/10'} rounded-lg font-mono text-lg font-bold text-yellow-500 hover:bg-white/10 transition-all duration-200 relative overflow-hidden group/copy"
								>
									<span class="relative z-10 flex items-center justify-center gap-2">
										{promo.code}
										{#if copiedCode === promo.code}
											<svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
										{:else}
											<svg class="w-5 h-5 text-gray-500 group-hover/copy:text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
										{/if}
									</span>
								</button>
							</div>
							{#if copiedCode === promo.code}
								<p class="text-xs text-green-400 flex items-center gap-1">
									<svg class="w-3 h-3" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path></svg>
									Copied to clipboard!
								</p>
							{/if}
						</div>

						<!-- Claim Button -->
						<a 
							href={promo.claimUrl}
							target="_blank"
							rel="noopener noreferrer"
							class="block w-full py-3 bg-yellow-500 hover:bg-yellow-400 text-black font-bold text-center rounded-lg transition-all duration-200 shadow-[0_0_15px_rgba(234,179,8,0.2)] hover:shadow-[0_0_25px_rgba(234,179,8,0.4)] transform hover:-translate-y-0.5 active:translate-y-0"
						>
							Claim Bonus
						</a>
					</div>
				</article>
			{/each}
		</div>

		<!-- Empty State -->
		{#if getFilteredCodes().length === 0}
			<div class="text-center py-16 space-y-3">
				<svg class="w-16 h-16 mx-auto text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
				</svg>
				<p class="text-gray-500 text-lg">No codes found in this category</p>
				<button 
					onclick={() => selectedFilter = 'all'}
					class="text-yellow-500 hover:text-yellow-400 font-medium"
				>
					View all codes
				</button>
			</div>
		{/if}
	</section>

	<!-- Info Section -->
	<section class="bg-[#1a1d24] border border-white/10 rounded-xl p-8">
		<div class="max-w-3xl mx-auto space-y-6">
			<h2 class="text-2xl font-display font-bold text-white text-center">
				How to Use <span class="text-yellow-500">Promo Codes</span>
			</h2>
			
			<div class="grid md:grid-cols-3 gap-6">
				<div class="text-center space-y-3">
					<div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center mx-auto">
						<span class="text-2xl font-bold text-yellow-500">1</span>
					</div>
					<h3 class="font-bold text-white">Copy Code</h3>
					<p class="text-sm text-gray-400">Click on any promo code to copy it to your clipboard</p>
				</div>
				
				<div class="text-center space-y-3">
					<div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center mx-auto">
						<span class="text-2xl font-bold text-yellow-500">2</span>
					</div>
					<h3 class="font-bold text-white">Visit Site</h3>
					<p class="text-sm text-gray-400">Click "Claim Bonus" to visit the site through our link</p>
				</div>
				
				<div class="text-center space-y-3">
					<div class="w-12 h-12 rounded-full bg-yellow-500/10 flex items-center justify-center mx-auto">
						<span class="text-2xl font-bold text-yellow-500">3</span>
					</div>
					<h3 class="font-bold text-white">Redeem</h3>
					<p class="text-sm text-gray-400">Paste the code during signup or in the promo section</p>
				</div>
			</div>

			<div class="pt-6 border-t border-white/10 space-y-3">
				<h3 class="font-bold text-white flex items-center gap-2">
					<svg class="w-5 h-5 text-yellow-500" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"></path></svg>
					Important Notes
				</h3>
				<ul class="text-sm text-gray-400 space-y-2">
					<li class="flex gap-2">
						<span class="text-yellow-500 shrink-0">•</span>
						<span>All promo codes are verified daily and tested by our team</span>
					</li>
					<li class="flex gap-2">
						<span class="text-yellow-500 shrink-0">•</span>
						<span>Most codes work for new users only - create a fresh account if needed</span>
					</li>
					<li class="flex gap-2">
						<span class="text-yellow-500 shrink-0">•</span>
						<span>Free bonuses may have wagering requirements before withdrawal</span>
					</li>
					<li class="flex gap-2">
						<span class="text-yellow-500 shrink-0">•</span>
						<span>You must be 18+ to use any CS2 skin gambling sites</span>
					</li>
				</ul>
			</div>
		</div>
	</section>
</div>
