<script>
	const posts = [
		{
			title: 'Best CS2 Case Opening Sites 2025: Ranked and Verified',
			slug: 'best-cs2-case-opening-sites-2025',
			excerpt:
				'Discover the top-rated and verified CS2 case opening sites for 2025. We rank the best platforms for fair odds, bonuses, and free cases.',
			date: '2025-01-02',
			readTime: '8 min read',
			image: '/blog/best-sites-header.jpg',
			category: 'Reviews'
		},
		{
			title: 'How to Get Free CS2 Skins in 2025 (No Deposit Required)',
			slug: 'how-to-get-free-cs2-skins-2025',
			excerpt:
				'Learn the legitimate ways to get free CS2 skins without spending a dime. From daily free cases to giveaways and efficient farming methods.',
			date: '2025-01-05',
			readTime: '6 min read',
			image: '/blog/free-skins-guide.jpg',
			category: 'Guides'
		},
		{
			title: 'The Ultimate List of Working CS2 Promo Codes (Updated Daily)',
			slug: 'working-cs2-promo-codes-daily',
			excerpt:
				"Don't miss out on free bonuses! We maintain an updated list of working promo codes for CSGOEmpire, Key-Drop, Clash.gg, and more.",
			date: '2025-01-08',
			readTime: '5 min read',
			image: '/blog/promo-codes-header.jpg',
			category: 'Promo Codes'
		},
		{
			title: 'Is CSGOEmpire Legit? A Comprehensive Review & Safety Guide',
			slug: 'is-csgoempire-legit-review',
			excerpt:
				'Is CSGOEmpire safe to use in 2025? We conduct a deep dive into their peer-to-peer trading system, withdrawal speed, and community reputation.',
			date: '2025-01-12',
			readTime: '10 min read',
			image: '/blog/empire-review.jpg',
			category: 'Reviews'
		},
		{
			title: 'CSGOEmpire vs. Clash.gg: Odds, Stats & Comparison (2025)',
			slug: 'csgoempire-vs-clashgg-case-odds',
			excerpt:
				'A deep dive analysis of CSGOEmpire vs. Clash.gg. We look at house edge, official Valve case odds, traffic stats, and withdrawal speeds to help you decide.',
			date: '2025-01-15',
			readTime: '7 min read',
			image: '/blog/empire-vs-clash.jpg',
			category: 'Comparisons'
		}
	];
</script>

<svelte:head>
	<title>CS2 Giveaways Blog | Guides, code & reviews</title>
	<meta
		name="description"
		content="Stay updated with the latest CS2 guides, site reviews, and promo codes. Learn how to win free skins and find the best sites to play on."
	/>
</svelte:head>

<div class="container mx-auto min-h-screen max-w-6xl px-4 py-12">
	<!-- Header -->
	<div class="mb-16 text-center">
		<span class="mb-4 inline-block font-bold tracking-wider text-yellow-500 uppercase">
			The Blog
		</span>
		<h1 class="font-display mb-6 text-4xl font-bold text-white md:text-5xl">
			Latest <span class="text-yellow-500">News & Guides</span>
		</h1>
		<p class="mx-auto max-w-2xl text-lg text-gray-400">
			Expert insights on CS2 trading, deep-dive reviews of case opening sites, and the latest
			strategies to build your inventory for free.
		</p>
	</div>

	<!-- Blog Grid -->
	<div class="grid gap-8 md:grid-cols-2 lg:grid-cols-3">
		{#each posts as post}
			<article class="group relative flex flex-col overflow-hidden rounded-2xl bg-[#1a1d24]">
				<!-- Hover Glow -->
				<div
					class="absolute -inset-1 rounded-2xl bg-gradient-to-r from-yellow-500/20 to-purple-500/20 opacity-0 blur-md transition-opacity duration-500 group-hover:opacity-100"
				></div>

				<!-- Content Container -->
				<div
					class="relative flex h-full flex-col overflow-hidden rounded-2xl border border-white/5 bg-[#1a1d24] transition-colors duration-300 group-hover:border-yellow-500/30"
				>
					<!-- Image Placeholder (since we don't have real images yet) -->
					<a
						href="/blog/{post.slug}"
						class="relative block aspect-video overflow-hidden bg-gray-800"
					>
						<img
							src={post.image}
							alt={post.title}
							class="h-full w-full object-cover transition-transform duration-500 group-hover:scale-110"
							onerror={(e) => (e.currentTarget.style.display = 'none')}
						/>
						<!-- Overlay -->
						<div
							class="absolute inset-0 bg-black/20 transition-colors group-hover:bg-black/0"
						></div>
					</a>

					<div class="flex flex-1 flex-col p-6">
						<!-- Meta -->
						<div class="mb-4 flex items-center justify-between text-xs text-gray-500">
							<span class="font-medium text-yellow-500">{post.category}</span>
							<span>{post.readTime}</span>
						</div>

						<!-- Title -->
						<a href="/blog/{post.slug}" class="mb-3 block">
							<h2
								class="font-display text-xl font-bold text-white transition-colors group-hover:text-yellow-500"
							>
								{post.title}
							</h2>
						</a>

						<!-- Excerpt -->
						<p class="mb-6 flex-1 text-sm leading-relaxed text-gray-400">
							{post.excerpt}
						</p>

						<!-- Read More -->
						<a
							href="/blog/{post.slug}"
							class="inline-flex items-center gap-2 text-sm font-bold text-white transition-colors hover:text-yellow-500"
						>
							Read Article
							<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M17 8l4 4m0 0l-4 4m4-4H3"
								/>
							</svg>
						</a>
					</div>
				</div>
			</article>
		{/each}
	</div>
</div>

<style>
	/* Custom scrollbar for this page if needed */
</style>
