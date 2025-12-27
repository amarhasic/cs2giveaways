<script>
	const promos = [
		{
			site: 'CSGOEmpire',
			type: 'Free Case',
			code: 'digipulation',
			reward: '$0.50 - $1400',
			status: 'Active',
			url: 'https://csgoempire.com/r/digipulation',
			color: '#FBAC18'
		},
		{
			site: 'Clash.gg',
			type: 'Deposit Bonus',
			code: 'DIGIPULATION',
			reward: '5% Bonus + 3 Cases',
			status: 'Active',
			url: 'https://clash.gg/r/DIGIPULATION',
			color: '#dc2626'
		},
		{
			site: 'Key-Drop',
			type: 'Free Cash',
			code: 'DIGIPULATION',
			reward: '$0.55 + 10% on Deposit',
			status: 'Active',
			url: 'https://kd.link/?code=DIGIPULATION',
			color: '#22c55e'
		},
		{
			site: 'Howl.gg',
			type: 'Welcome Case',
			code: 'dpl',
			reward: 'Free Case',
			status: 'Active',
			url: 'https://howl.gg/r/dpl',
			color: '#7c3aed'
		},
		{
			site: 'DatDrop',
			type: 'Deposit Bonus',
			code: 'digipulation',
			reward: '+5% Deposit Bonus',
			status: 'Active',
			url: 'https://datdrop.com/p/digipulation',
			color: '#3b82f6'
		},
		{
			site: 'Farmskins',
			type: 'Free Balance',
			code: 'digipulation',
			reward: '$0.90 Free',
			status: 'Active',
			url: 'https://farmskins.com/ref-digipulation',
			color: '#ec4899'
		}
	];

	async function copyCode(code) {
		try {
			await navigator.clipboard.writeText(code);
			alert('Code copied: ' + code);
		} catch (err) {
			console.error('Failed to copy', err);
		}
	}

	let currentDate = new Date().toLocaleDateString('en-US', {
		weekday: 'long',
		year: 'numeric',
		month: 'long',
		day: 'numeric'
	});

	let jsonLd = {
		'@context': 'https://schema.org',
		'@type': 'ItemList',
		name: 'Top CS2 Promo Codes 2026',
		itemListElement: promos.map((promo, index) => ({
			'@type': 'ListItem',
			position: index + 1,
			item: {
				'@type': 'DiscountCode',
				codeValue: promo.code,
				description: `Get ${promo.reward} at ${promo.site}`,
				provider: {
					'@type': 'Organization',
					name: promo.site,
					url: promo.url
				}
			}
		}))
	};
</script>

<svelte:head>
	<title>Working CS2 Promo Codes (Daily Updated List 2026) | CS2 Giveaways</title>
	<meta
		name="description"
		content="The ultimate list of working CS2 promo codes for 2026. Claim free cases, deposit bonuses, and free balance on Empire, Key-Drop, Clash.gg, and more."
	/>
	{@html `<script type="application/ld+json">${JSON.stringify(jsonLd)}</script>`}
</svelte:head>

<div class="relative min-h-screen bg-[#0f1115] pt-12 pb-24">
	<!-- Background Elements -->
	<div class="pointer-events-none absolute inset-0 overflow-hidden">
		<div
			class="absolute top-0 left-0 -translate-y-1/2 opacity-10 blur-[100px]"
			style="width: 600px; height: 600px; background: radial-gradient(circle, #22c55e 0%, transparent 70%);"
		></div>
	</div>

	<div class="container mx-auto max-w-4xl px-4">
		<!-- Article Header -->
		<header class="mb-12 text-center">
			<nav class="mb-8 flex justify-center text-sm text-gray-400">
				<a href="/blog" class="hover:text-white">Blog</a>
				<span class="mx-2">/</span>
				<span class="text-white">Promo Codes</span>
			</nav>
			<div
				class="mb-4 inline-flex items-center gap-2 rounded-full border border-green-500/20 bg-green-500/10 px-4 py-1 text-sm font-bold text-green-500"
			>
				<span class="relative flex h-2 w-2">
					<span
						class="absolute inline-flex h-full w-full animate-ping rounded-full bg-green-400 opacity-75"
					></span>
					<span class="relative inline-flex h-2 w-2 rounded-full bg-green-500"></span>
				</span>
				Updated: <time datetime={new Date().toISOString().split('T')[0]}>{currentDate}</time>
			</div>
			<h1
				class="font-display mb-6 text-4xl font-bold text-white md:text-5xl lg:text-6xl"
				itemprop="headline"
			>
				Ultimate List of <span class="text-green-500">Working CS2 Codes</span>
			</h1>
			<p class="mx-auto max-w-2xl text-xl text-gray-400">
				Don't waste time with expired codes. We verify these promo codes daily to ensure you get
				your free cases and bonuses instantly.
			</p>
			<div class="mt-6 flex items-center justify-center gap-6 text-sm text-gray-500">
				<div class="flex items-center gap-2">
					<svg class="h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
						/>
					</svg>
					Updated Daily
				</div>
				<div class="flex items-center gap-2">
					<svg class="h-5 w-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M13 10V3L4 14h7v7l9-11h-7z"
						/>
					</svg>
					High Success Rate
				</div>
			</div>
		</header>

		<!-- Content -->
		<article
			class="prose prose-invert prose-lg mx-auto max-w-none"
			itemscope
			itemtype="https://schema.org/Article"
		>
			<meta itemprop="author" content="CS2 Giveaways" />
			<meta itemprop="publisher" content="CS2 Giveaways" />
			<meta itemprop="datePublished" content="2026-01-08" />
			<meta itemprop="image" content="https://cs2giveaways.com/blog/promo-codes-header.jpg" />
			<!-- Featured Image -->
			<div class="not-prose mb-12 overflow-hidden rounded-2xl border border-white/10 shadow-2xl">
				<div class="relative flex aspect-video items-center justify-center bg-[#1a1d24]">
					<img
						src="/blog/promo-codes-header.jpg"
						alt="CS2 Promo Codes Header"
						class="h-full w-full object-cover opacity-80"
						onerror={(e) => {
							const img = e.currentTarget;
							img.src = '/logo.png';
							img.classList.add('p-20', 'object-contain');
						}}
					/>
				</div>
			</div>

			<p class="lead text-xl text-gray-300">
				Finding working promo codes for CS2 gambling and case opening sites can be a frustrating
				experience. Too many sites list codes that expired months ago.
			</p>
			<p>
				Below is our master list of verified, active codes for the most popular sites in 2025. These
				are referral codes that guarantee free signup bonuses.
			</p>

			<!-- Promo Codes List -->
			<div class="not-prose my-12 space-y-4">
				{#each promos as promo}
					<div
						class="group relative flex flex-col items-center gap-4 overflow-hidden rounded-xl border border-white/10 bg-[#1a1d24] p-4 transition-all hover:border-green-500/30 md:flex-row md:p-6"
						itemscope
						itemtype="https://schema.org/DiscountCode"
					>
						<!-- Status Indicator -->
						<div
							class="absolute top-0 right-0 rounded-bl-xl bg-green-500/10 px-3 py-1 text-xs font-bold text-green-500"
						>
							{promo.status}
						</div>

						<!-- Site Info -->
						<div class="flex-1 text-center md:text-left">
							<h3
								class="flex items-center justify-center gap-2 text-xl font-bold text-white md:justify-start"
								itemprop="provider"
							>
								{promo.site}
								<span class="rounded bg-white/10 px-2 py-0.5 text-xs font-normal text-gray-400"
									>{promo.type}</span
								>
							</h3>
							<p class="mt-1 font-bold text-yellow-500" itemprop="description">{promo.reward}</p>
						</div>

						<!-- Code & Action -->
						<div class="flex w-full flex-col gap-3 md:w-auto md:flex-row md:items-center">
							<button
								onclick={() => copyCode(promo.code)}
								class="group/btn relative flex items-center justify-center gap-2 rounded-lg border-2 border-dashed border-gray-600 bg-black/20 px-4 py-2 font-mono font-bold text-gray-300 transition-colors hover:border-white hover:text-white"
							>
								<code itemprop="codeValue">{promo.code}</code>
								<svg
									class="h-4 w-4 opacity-0 transition-opacity group-hover/btn:opacity-100"
									fill="none"
									stroke="currentColor"
									viewBox="0 0 24 24"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"
									/>
								</svg>
							</button>
							<a
								href={promo.url}
								target="_blank"
								class="rounded-lg px-6 py-2.5 text-center font-bold whitespace-nowrap text-black transition-transform hover:scale-105"
								style="background-color: {promo.color};"
							>
								Claim Now
							</a>
						</div>
					</div>
				{/each}
			</div>

			<h2>How to Use These Codes</h2>
			<ol>
				<li><strong>Click "Claim Now":</strong> This will open the official site in a new tab.</li>
				<li><strong>Sign In:</strong> Log in with your Steam account.</li>
				<li>
					<strong>Find "Promo Code" or "Affiliates":</strong> Usually located in the top menu or user
					profile.
				</li>
				<li><strong>Enter the Code:</strong> Paste the code you copied from our list.</li>
				<li>
					<strong>Enjoy:</strong> Your free cases or bonus balance will be credited instantly.
				</li>
			</ol>

			<div class="mt-12 rounded-xl border border-blue-500/20 bg-blue-900/10 p-6">
				<h3 class="mb-2 flex items-center gap-2 text-lg font-bold text-white">
					<svg class="h-5 w-5 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
						/>
					</svg>
					Pro Tip
				</h3>
				<p class="m-0 text-sm text-gray-300">
					Bookmark this page! We update the status of these codes daily using our automated checker.
					If a code stops working, we remove it immediately.
				</p>
			</div>
		</article>
	</div>
</div>
