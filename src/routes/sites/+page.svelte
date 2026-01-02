<script>
	const sites = [
		{
			id: 'csgoempire',
			name: 'CSGOEmpire',
			tagline: 'The OG CS2 Casino',
			rating: 4.8,
			image: '/csgoempire.jpg',
			affiliateUrl: 'https://csgoempire.com/r/digipulation',
			features: ['Roulette', 'Coinflip', 'Cases', 'Case Battles', 'Match Betting'],
			bonuses: ['Daily Chests', 'Chat Rain', 'Free Cases'],
			pros: [
				'Stable and polished platform',
				'Reliable deposits and withdrawals',
				'Responsive support team',
				'Long-standing reputation',
				'Skin-based economy feels legit'
			],
			cons: [
				'Clear house edge on games',
				'Long sessions can drain balance',
				'Risk of chasing losses'
			],
			review: `CSGOEmpire feels like the default "OG" CS2 casino: roulette, coinflip, cases, case battles, match betting, daily chests and constant rain in chat, all built around skins with optional crypto and fiat on top. The site feels stable and polished, deposits and withdrawals go through reliably, and support actually responds and helps instead of auto-replying, which makes it the one that feels most trustworthy long-term.

When using it, it never felt like some shady pop-up casino – the license info, long history, and the fact that the economy is mostly skin-based gives it a more legit vibe. The downside is that it is still a proper casino with a clear house edge, so long sessions can eat a balance fast and there is always the risk of chasing losses; it is great for grinding daily rewards, but not for "printing money".`,
			verdict:
				'Best for players who want a reliable, trustworthy platform with consistent daily rewards.',
			color: '#dc2626'
		},
		{
			id: 'duel',
			name: 'Duel',
			tagline: 'Fairness & High RTP Revolution',
			rating: 4.9,
			image: '/duel-logo.png',
			affiliateUrl: 'https://duel.com/r/digi',
			features: [
				'High RTP (99.9%)',
				'Zero-Edge Games',
				'PvP Markets',
				'Provably Fair',
				'Live Casino'
			],
			bonuses: ['Instant Rakeback', 'No Wager Requirements', 'Daily Rewards'],
			pros: [
				'Industry-leading 99.9% RTP',
				'No wagering requirements on rakeback',
				'100% real wagers, no fake volume',
				'Fully provably fair & transparent',
				'Backed by experienced ownership'
			],
			cons: [
				'Newer platform than legacy sites',
				'Focus on raw value over flashy minigames',
				'Zero-edge library still growing'
			],
			review: `Duel is built around fixing the core problems in online gambling by prioritizing fairness, transparency, and actual player value. All wagers on the platform use real money, with no fake bets or staged activity, and the site deliberately avoids dark-UX patterns and misleading bonus mechanics. With RTP sitting around 99.9% and a clear goal of reaching 100% on selected games, Duel operates with a far lower house edge than most mainstream casinos.
			
Instead of complicated bonuses full of conditions, it offers instant rakeback equal to roughly 50% of the house edge on slots, reducing losses immediately with no strings attached. Rather than chasing short-term profit, Duel is structured around treating users fairly and improving industry standards, positioning itself as a cleaner, more transparent alternative to the typical casino model.`,
			verdict: 'The new gold standard for fairness and high RTP, perfect for smart players.',
			color: '#8b5cf6'
		},
		{
			id: 'keydrop',
			name: 'Key-Drop',
			tagline: 'Turbo-Charged Case Opening',
			rating: 4.5,
			image: '/keydrop.jpg',
			affiliateUrl: 'https://kd.link/?code=DIGIPULATION',
			features: ['Custom Cases', 'Case Battles', 'Upgrades', 'Contracts'],
			bonuses: ['Daily Free Case', 'Promo Codes', 'Events'],
			pros: [
				'Tons of custom cases',
				'Super easy and fun to use',
				'Clean interface',
				'Daily free stuff',
				'Non-stop promo codes and events'
			],
			cons: [
				'Stricter verification for big wins',
				'Some items occasionally unavailable',
				'Less friendly when cashing out large amounts'
			],
			review: `Key-Drop is basically "Valve cases but turbo-charged": tons of custom cases, case battles, upgrades and non-stop promo codes and events that constantly tempt you to open "just one more". From a user point of view it is super easy and fun to use, with a clean interface, daily free stuff, and withdrawals that usually work fine when you are not trying to cash out huge amounts at once.

However, once you start winning bigger or try to pull out better skins, the site becomes noticeably more strict: verification kicks in, some items are suddenly "unavailable", and the whole thing feels a lot less friendly. The overall impression is that it is a massive, functional platform that is great for milking bonuses and low-stakes fun, but not somewhere to park serious value or expect zero friction when you get on a lucky streak.`,
			verdict:
				'Perfect for bonus hunters and casual case openers looking for low-stakes entertainment.',
			color: '#2563eb'
		},
		{
			id: 'howl',
			name: 'Howl.gg',
			tagline: 'Bonus-Heavy Rewards Platform',
			rating: 4.0,
			image: '/howl.jpg',
			affiliateUrl: 'https://howl.gg/r/dpl',
			features: ['Upgrader', 'Crash', 'Cases', 'Battles'],
			bonuses: ['Deposit Bonuses', 'Daily Free Stuff', 'VIP Rewards'],
			pros: [
				'Juicy deposit bonuses',
				'Daily free rewards',
				'VIP rewards program',
				'Active Discord community',
				'Enjoyable rewards ladder'
			],
			cons: ['Strict KYC for cashouts', 'Delays on withdrawals', 'Conditions on bonus extraction'],
			review: `Howl.gg feels like the classic "bonus-heavy" CS2 site: juicy deposit bonuses, daily free stuff, VIP rewards, and a lot of social hype in chat and on Discord. Game-mode wise you get the usual suspects (upgrader, crash, cases, etc.) and the interface is nice enough that grinding the rewards ladder can actually be enjoyable.

The catch is that the site becomes noticeably more picky the moment you try to cash out decent wins: KYC requests, account checks, and delays that feel way longer than they should. It does not come across as a straight-up fake site, but it definitely feels like one where you never deposit money you are not prepared to have locked up or slowly extracted through conditions and verification hoops.`,
			verdict: 'Good for grinding bonuses, but be cautious with larger deposits.',
			color: '#7c3aed'
		},
		{
			id: 'clash',
			name: 'Clash.gg',
			tagline: 'Modern & Crypto-Friendly',
			rating: 4.6,
			image: '/clash.jpg',
			affiliateUrl: 'https://clash.gg/r/DIGIPULATION',
			features: ['Case Battles', 'Upgrader', 'Plinko', 'Provably Fair'],
			bonuses: ['3 Free Cases', 'Crypto Bonuses', 'Referral Rewards'],
			pros: [
				'Modern, clean UI',
				'Provably fair games',
				'Crypto-friendly',
				'Smooth deposits and cashouts',
				'PayPal support'
			],
			cons: [
				'Occasional lag and UI glitches',
				'KYC for larger amounts',
				'House edge still applies'
			],
			review: `Clash.gg gives a more "modern" vibe with a clean UI, case battles, upgrader, Plinko and other games, plus a heavy focus on being provably fair and crypto-friendly. Using it, deposits and cashouts feel smoother than on a lot of competitors, especially if you are comfortable with crypto or PayPal and keep things reasonable.

It is still a casino: you can run hot and withdraw quickly, or you can get destroyed by variance in a handful of rounds, and the house edge is always there. There are also occasional annoyances like lag, minor UI glitches and the usual KYC checks if you start moving larger amounts, but overall it feels like one of the more balanced, well-run options in this niche.`,
			verdict: 'One of the most balanced and professional options for crypto users.',
			color: '#059669'
		},
		{
			id: '500casino',
			name: '500 Casino',
			tagline: 'Massive Deposit Bonuses',
			rating: 4.3,
			image: '/500casino.jpg',
			affiliateUrl: 'https://500.casino/r/DIGIPULATION',
			features: ['Slots', 'Live Casino', 'Sports Betting', 'Crash', 'Dice'],
			bonuses: ['100% Deposit Bonus up to $1k', '50 Free Spins', 'VIP Program'],
			pros: [
				'Huge welcome bonus package',
				'Wide variety of games',
				'Sports betting included',
				'Crypto and fiat support',
				'Active VIP program'
			],
			cons: [
				'Wagering requirements on bonuses',
				'Can feel overwhelming for new users',
				'Support response times vary'
			],
			review: `500 Casino positions itself as a full-scale online casino with a massive welcome offer: 100% deposit bonus up to $1,000 plus 50 free spins right out of the gate. Beyond CS2-related content, you get access to slots, live casino games, sports betting, crash, dice, and pretty much everything you would expect from a modern gambling platform.

The sheer variety is both a strength and a weakness: there is always something to play, but it can feel overwhelming if you are just looking for simple case opening or CS2-focused gambling. The bonuses are genuinely attractive, though the wagering requirements mean you will need to play through a fair amount before extracting any value. Overall, it is a solid choice if you want an all-in-one gambling hub with a big bonus to start.`,
			verdict: 'Best for players who want a full casino experience with a massive welcome bonus.',
			color: '#10b981'
		},
		{
			id: 'roobet',
			name: 'Roobet',
			tagline: 'Crypto Casino Pioneer',
			rating: 4.4,
			image: '/roobet.jpg',
			affiliateUrl: 'https://roobet.com/?ref=digipulation',
			features: ['Crash', 'Roulette', 'Mines', 'Slots', 'Live Casino'],
			bonuses: ['Daily Rewards', 'Weekly Rewards', 'Monthly Rewards', 'Rakeback'],
			pros: [
				'Established crypto casino brand',
				'Consistent rakeback system',
				'Daily, weekly, and monthly rewards',
				'Provably fair games',
				'Smooth user experience'
			],
			cons: [
				'Crypto-only deposits',
				'Geo-restrictions in some regions',
				'High volume needed for best rakeback'
			],
			review: `Roobet has carved out a solid reputation as one of the OG crypto casinos, known for its Crash game, provably fair mechanics, and a steady stream of rewards. The rakeback system is genuinely useful: you get a percentage back on your plays daily, weekly, and monthly, which can add up if you are a consistent player.

The catch is that it is crypto-only, so if you prefer fiat deposits, you will need to look elsewhere. There are also geo-restrictions depending on where you live, so check availability first. For those who are already in the crypto gambling scene, Roobet feels like a reliable, well-established platform where the rewards structure actually makes sense for regular players.`,
			verdict: 'Ideal for crypto gamblers who value consistent rakeback and rewards.',
			color: '#f59e0b'
		},
		{
			id: 'gamdom',
			name: 'Gamdom',
			tagline: 'Rakeback King',
			rating: 4.5,
			image: '/gamdom.jpg',
			affiliateUrl: 'https://gamdom.com/r/digipulation',
			features: ['Crash', 'Roulette', 'HiLo', 'Slots', 'Sports Betting'],
			bonuses: ['15% Instant Rakeback (7 days)', 'Up to 60% Rakeback', 'Rain', 'Rewards'],
			pros: [
				'Industry-leading rakeback rates',
				'15% instant rakeback for new users',
				'Up to 60% rakeback for high volume',
				'Diverse game selection',
				'Active community with rain'
			],
			cons: [
				'Rakeback tiers require significant volume',
				'Interface can feel cluttered',
				'Crypto-focused (limited fiat options)'
			],
			review: `Gamdom stands out with what is arguably the most aggressive rakeback program in the space: 15% instant rakeback for your first 7 days, scaling up to an insane 60% for high-volume players. If you are the type who plays consistently and wants to minimize the house edge over time, Gamdom's structure is genuinely compelling.

Beyond rakeback, you get the usual suspects: crash, roulette, HiLo, slots, and even sports betting. The community is active with regular rain in chat, and the overall vibe is more "grinder-friendly" than most competitors. The downside is that hitting those top rakeback tiers requires serious volume, and the interface can feel a bit cluttered compared to cleaner alternatives.`,
			verdict: 'The go-to platform for serious grinders who want maximum rakeback.',
			color: '#8b5cf6'
		},
		{
			id: 'farmskins',
			name: 'Farmskins',
			tagline: 'Case Opening Paradise',
			rating: 4.2,
			image: '/farmskins.jpg',
			affiliateUrl: 'https://farmskins.com/ref-digipulation',
			features: ['Case Opening', 'Upgrades', 'Contracts', 'Case Battles'],
			bonuses: ['$1 Free Balance', 'Daily Free Cases', 'Promo Codes'],
			pros: [
				'Free $1 balance on signup',
				'Daily free cases',
				'Huge variety of cases',
				'Skin-focused economy',
				'Regular promo codes'
			],
			cons: [
				'Withdrawal minimums apply',
				'Some cases have low value odds',
				'KYC for larger cashouts'
			],
			review: `Farmskins is a dedicated case opening site that hooks you immediately with $1 free balance and daily free cases. The platform is laser-focused on what it does: tons of custom cases, upgrades, contracts, and case battles, all built around the CS2 skin economy that gives it a more "authentic" feel than pure casino sites.

For bonus hunters and casual case openers, it is a great place to farm free value from promo codes and daily rewards. The downside is that withdrawal minimums mean you need to build up some balance before cashing out, and like most case sites, the odds on cheaper cases are not in your favor. Treat it as a fun grind rather than a profit machine.`,
			verdict: 'Perfect for case opening enthusiasts who want daily free spins at skins.',
			color: '#ec4899'
		},
		{
			id: 'casehug',
			name: 'Casehug',
			tagline: 'Generous Top-Up Bonuses',
			rating: 4.0,
			image: '/casehug.jpg',
			affiliateUrl: 'https://casehug.com/r/DIGI',
			features: ['Case Opening', 'Upgrades', 'Case Battles', 'Contracts'],
			bonuses: ['$1 Free on Signup', '20% on Every Top-Up', 'Promo Codes'],
			pros: [
				'$1 free bonus on signup',
				'20% extra on every deposit',
				'Clean interface',
				'Regular promotions',
				'Quick withdrawals for small amounts'
			],
			cons: [
				'Smaller site with less traffic',
				'Limited game variety',
				'Less established reputation'
			],
			review: `Casehug positions itself as a straightforward case opening site with a focus on generous bonuses: $1 free on signup and 20% extra on every top-up, which is one of the better ongoing deposit bonuses in the space. The interface is clean and the core experience (cases, upgrades, battles) works as expected.

Being a smaller site, it has less traffic than giants like Key-Drop, which means case battles might take longer to fill and the community feels quieter. That said, for solo case opening and grinding promo codes, it delivers solid value. Just keep expectations realistic since it is not as battle-tested as the bigger names.`,
			verdict: 'Good for players who value ongoing deposit bonuses over site prestige.',
			color: '#06b6d4'
		},
		{
			id: 'csgoluck',
			name: 'CSGOLuck',
			tagline: 'Classic CS2 Gambling',
			rating: 3.8,
			image: '/csgoluck.jpg',
			affiliateUrl: 'https://csgoluck.com/rewards',
			features: ['Roulette', 'Crash', 'Coinflip', 'Jackpot', 'Cases'],
			bonuses: ['Rewards Program', 'Promo Codes', 'Referral Bonuses'],
			pros: [
				'Classic CS2 gambling games',
				'Simple and familiar interface',
				'Active rewards program',
				'Skin deposits supported',
				'Established in the scene'
			],
			cons: ['Less modern than competitors', 'Smaller community', 'Fewer unique features'],
			review: `CSGOLuck offers the classic CS2 gambling experience: roulette, crash, coinflip, jackpot, and cases. If you have been in the scene for a while, the interface will feel instantly familiar. It does not try to reinvent the wheel; it just delivers the core games that CS2 gamblers have always played.

The rewards program provides ongoing value, and skin deposits make it accessible for those who want to gamble their inventory directly. The downside is that it feels less modern than newer competitors, and the community is smaller, which can mean longer waits for multiplayer games like jackpot. Solid for a quick, no-frills gambling session.`,
			verdict: 'A reliable classic for players who prefer traditional CS2 gambling games.',
			color: '#64748b'
		}
	];

	function getStarArray(rating) {
		const stars = [];
		const fullStars = Math.floor(rating);
		const hasHalf = rating % 1 >= 0.5;

		for (let i = 0; i < fullStars; i++) {
			stars.push('full');
		}
		if (hasHalf) {
			stars.push('half');
		}
		while (stars.length < 5) {
			stars.push('empty');
		}
		return stars;
	}

	// SEO Data
	const schemaOrg = {
		'@context': 'https://schema.org',
		'@type': 'ItemList',
		itemListElement: sites.map((site, index) => ({
			'@type': 'ListItem',
			position: index + 1,
			item: {
				'@type': 'Review',
				name: `${site.name} Review`,
				itemReviewed: {
					'@type': 'Organization',
					name: site.name,
					image: `https://cs2giveaways.com${site.image}`
				},
				reviewRating: {
					'@type': 'Rating',
					ratingValue: site.rating,
					bestRating: '5',
					worstRating: '1'
				},
				author: {
					'@type': 'Organization',
					name: 'CS2 Giveaways'
				},
				reviewBody: site.review
			}
		}))
	};
</script>

<svelte:head>
	<title>Top 10 Trusted CS2 Gambling Sites 2026 | Reviews & Promo Codes</title>
	<meta
		name="description"
		content="Detailed reviews of the best CS2 gambling sites including Empire, Clash.gg, and Key-Drop. Tested for fairness, bonuses, and withdrawal speed. Claim exclusive codes."
	/>
	<meta
		name="keywords"
		content="cs2 gambling sites, best csgo sites, cs2 betting reviews, verified cs2 casinos, trusted skin sites"
	/>

	<!-- JSON-LD Structured Data -->
	{@html `<script type="application/ld+json">${JSON.stringify(schemaOrg)}</script>`}
</svelte:head>

<div class="container mx-auto max-w-6xl space-y-16 px-4 py-12">
	<!-- Hero Section -->
	<section class="space-y-6 text-center">
		<div
			class="inline-flex items-center gap-2 rounded-full border border-yellow-500/20 bg-yellow-500/10 px-4 py-2 text-sm font-medium text-yellow-500"
		>
			<svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20">
				<path
					d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
				/>
			</svg>
			<span>Honest Reviews • Updated January 2026</span>
		</div>

		<h1 class="font-display text-4xl font-bold text-white md:text-5xl lg:text-6xl">
			Top <span class="text-yellow-500">CS2 Sites</span> Reviewed
		</h1>

		<p class="mx-auto max-w-3xl text-lg leading-relaxed text-gray-400">
			We've tested and reviewed the most popular CS2 gambling and case opening sites. Here's our
			honest take on each platform, including the good, the bad, and everything in between.
		</p>
	</section>

	<!-- Quick Summary -->
	<section
		class="rounded-2xl border border-white/10 bg-gradient-to-br from-[#1a1d24] to-[#12141a] p-6 md:p-8"
	>
		<h2 class="font-display mb-6 flex items-center gap-3 text-xl font-bold text-white">
			<svg class="h-6 w-6 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M13 10V3L4 14h7v7l9-11h-7z"
				/>
			</svg>
			Quick Summary
		</h2>

		<div class="prose prose-invert max-w-none leading-relaxed text-gray-300">
			<p>
				From actual usage, <strong class="text-white">CSGOEmpire</strong> and
				<strong class="text-white">Clash.gg</strong>
				feel like the "safer" places to play when it comes to consistency, support and general professionalism.
				<strong class="text-white">Key-Drop</strong> and <strong class="text-white">Howl.gg</strong>
				are the ones to hit when chasing aggressive bonuses and free cases, but they also feel more fragile
				the moment you start winning big, so they are better treated as short-term value farms rather
				than long-term banks for skins or cash.
			</p>
		</div>
	</section>

	<!-- Site Reviews -->
	<section class="space-y-8">
		<div class="flex items-center gap-4">
			<div class="h-px flex-1 bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
			<span class="text-sm tracking-wider text-gray-600 uppercase">Detailed Reviews</span>
			<div class="h-px flex-1 bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
		</div>

		{#each sites as site, index}
			<article
				id={site.id}
				class="group relative overflow-hidden rounded-2xl border border-white/10 bg-gradient-to-br from-[#1a1d24] to-[#12141a] transition-all duration-300 hover:border-white/20"
			>
				<!-- Accent bar -->
				<div class="absolute top-0 right-0 left-0 h-1" style="background: {site.color};"></div>

				<div class="p-6 md:p-8">
					<!-- Header -->
					<div class="mb-8 flex flex-col gap-6 lg:flex-row">
						<!-- Site Image -->
						<div class="flex-shrink-0 lg:w-64">
							<div
								class="relative aspect-video overflow-hidden rounded-xl border border-white/10 lg:aspect-square"
							>
								<img
									src={site.image}
									alt="{site.name} Screenshot"
									class="h-full w-full object-cover"
									loading="lazy"
								/>
								<div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
								<div class="absolute right-3 bottom-3 left-3">
									<span
										class="inline-block rounded-full px-3 py-1 text-xs font-bold text-white"
										style="background: {site.color};"
									>
										#{index + 1} Ranked
									</span>
								</div>
							</div>
						</div>

						<!-- Site Info -->
						<div class="flex-1 space-y-4">
							<div class="flex flex-wrap items-start justify-between gap-4">
								<div>
									<h2 class="font-display mb-1 text-2xl font-bold text-white md:text-3xl">
										{site.name}
									</h2>
									<p class="text-gray-400">{site.tagline}</p>
								</div>

								<!-- Rating -->
								<div class="flex items-center gap-2 rounded-full bg-white/5 px-4 py-2">
									<div class="flex">
										{#each getStarArray(site.rating) as star, starIndex}
											{#if star === 'full'}
												<svg
													class="h-5 w-5 text-yellow-500"
													fill="currentColor"
													viewBox="0 0 20 20"
												>
													<path
														d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
													/>
												</svg>
											{:else if star === 'half'}
												<svg
													class="h-5 w-5 text-yellow-500"
													fill="currentColor"
													viewBox="0 0 20 20"
												>
													<defs>
														<linearGradient id="half-{site.id}-{starIndex}">
															<stop offset="50%" stop-color="currentColor" />
															<stop offset="50%" stop-color="#374151" />
														</linearGradient>
													</defs>
													<path
														fill="url(#half-{site.id}-{starIndex})"
														d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
													/>
												</svg>
											{:else}
												<svg class="h-5 w-5 text-gray-600" fill="currentColor" viewBox="0 0 20 20">
													<path
														d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
													/>
												</svg>
											{/if}
										{/each}
									</div>
									<span class="font-bold text-white">{site.rating}</span>
								</div>
							</div>

							<!-- Features & Bonuses -->
							<div class="flex flex-wrap gap-2">
								{#each site.features as feature}
									<span
										class="rounded-full border border-white/10 bg-white/5 px-3 py-1 text-xs text-gray-300"
									>
										{feature}
									</span>
								{/each}
							</div>

							<div class="flex flex-wrap gap-2">
								{#each site.bonuses as bonus}
									<span
										class="rounded-full border border-yellow-500/20 bg-yellow-500/10 px-3 py-1 text-xs font-medium text-yellow-500"
									>
										🎁 {bonus}
									</span>
								{/each}
							</div>
						</div>
					</div>

					<!-- Review Content -->
					<div class="space-y-6">
						<div class="prose prose-invert max-w-none">
							{#each site.review.split('\n\n') as paragraph}
								<p class="leading-relaxed text-gray-300">{paragraph}</p>
							{/each}
						</div>

						<!-- Pros & Cons -->
						<div class="grid gap-4 md:grid-cols-2">
							<!-- Pros -->
							<div class="rounded-xl border border-green-500/20 bg-green-500/5 p-5">
								<h3 class="mb-4 flex items-center gap-2 font-semibold text-green-400">
									<svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
										/>
									</svg>
									Pros
								</h3>
								<ul class="space-y-2">
									{#each site.pros as pro}
										<li class="flex items-start gap-2 text-sm text-gray-300">
											<svg
												class="mt-0.5 h-4 w-4 flex-shrink-0 text-green-400"
												fill="none"
												stroke="currentColor"
												viewBox="0 0 24 24"
											>
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													stroke-width="2"
													d="M5 13l4 4L19 7"
												/>
											</svg>
											{pro}
										</li>
									{/each}
								</ul>
							</div>

							<!-- Cons -->
							<div class="rounded-xl border border-red-500/20 bg-red-500/5 p-5">
								<h3 class="mb-4 flex items-center gap-2 font-semibold text-red-400">
									<svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
										/>
									</svg>
									Cons
								</h3>
								<ul class="space-y-2">
									{#each site.cons as con}
										<li class="flex items-start gap-2 text-sm text-gray-300">
											<svg
												class="mt-0.5 h-4 w-4 flex-shrink-0 text-red-400"
												fill="none"
												stroke="currentColor"
												viewBox="0 0 24 24"
											>
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													stroke-width="2"
													d="M6 18L18 6M6 6l12 12"
												/>
											</svg>
											{con}
										</li>
									{/each}
								</ul>
							</div>
						</div>

						<!-- Verdict & CTA -->
						<div
							class="flex flex-col items-start justify-between gap-4 border-t border-white/10 pt-4 sm:flex-row sm:items-center"
						>
							<div class="flex items-start gap-3">
								<div
									class="flex h-10 w-10 flex-shrink-0 items-center justify-center rounded-full"
									style="background: {site.color}20; border: 1px solid {site.color}40;"
								>
									<svg
										class="h-5 w-5"
										style="color: {site.color};"
										fill="none"
										stroke="currentColor"
										viewBox="0 0 24 24"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"
										/>
									</svg>
								</div>
								<div>
									<p class="mb-1 text-xs tracking-wider text-gray-500 uppercase">Our Verdict</p>
									<p class="font-medium text-white">{site.verdict}</p>
								</div>
							</div>

							<a
								href={site.affiliateUrl}
								target="_blank"
								rel="noopener noreferrer"
								class="inline-flex items-center gap-2 rounded-full px-6 py-3 font-bold text-white transition-all duration-300 hover:scale-105 hover:shadow-lg"
								style="background: linear-gradient(135deg, {site.color}, {site.color}dd); box-shadow: 0 4px 20px {site.color}40;"
							>
								Visit {site.name}
								<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"
									/>
								</svg>
							</a>
						</div>
					</div>
				</div>
			</article>
		{/each}
	</section>

	<!-- Disclaimer -->
	<section class="rounded-2xl border border-yellow-500/20 bg-yellow-500/5 p-6 md:p-8">
		<div class="flex items-start gap-4">
			<div
				class="flex h-12 w-12 flex-shrink-0 items-center justify-center rounded-full bg-yellow-500/20"
			>
				<svg class="h-6 w-6 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"
					/>
				</svg>
			</div>
			<div>
				<h3 class="mb-2 text-lg font-bold text-yellow-500">Important Disclaimer</h3>
				<p class="text-sm leading-relaxed text-gray-400">
					Gambling involves risk. All sites reviewed here have a house edge, meaning the odds are
					always in favor of the house over time. Never gamble with money you can't afford to lose.
					These reviews are based on personal experience and may not reflect your own. Some links on
					this page are affiliate links, meaning we may earn a commission if you sign up through
					them. This does not affect our reviews or ratings. Please gamble responsibly and check
					your local laws regarding online gambling.
				</p>
			</div>
		</div>
	</section>

	<!-- Quick Navigation -->
	<section class="sticky bottom-4 z-40">
		<div
			class="mx-auto max-w-fit rounded-full border border-white/10 bg-[#1a1d24]/95 p-2 shadow-2xl backdrop-blur-md"
		>
			<div class="flex items-center gap-1">
				<span class="hidden px-3 text-xs text-gray-500 sm:block">Jump to:</span>
				{#each sites as site}
					<a
						href="#{site.id}"
						class="rounded-full px-3 py-2 text-xs font-medium text-gray-400 transition-colors hover:bg-white/10 hover:text-white"
					>
						{site.name}
					</a>
				{/each}
			</div>
		</div>
	</section>
</div>

<style>
	:global(html) {
		scroll-behavior: smooth;
	}
</style>
