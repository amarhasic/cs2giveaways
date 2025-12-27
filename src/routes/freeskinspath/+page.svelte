<script>
	import { onMount } from 'svelte';

	let visibleSteps = $state([]);

	const offers = [
		{
			id: 1,
			site: 'CSGOEmpire',
			reward: '1 Free Case',
			value: 0.5,
			description: 'The OG CS2 gambling site with instant case rewards.',
			url: 'https://csgoempire.com/r/digipulation',
			code: 'digipulation',
			accentColor: '#dc2626',
			tag: 'INSTANT'
		},
		{
			id: 2,
			site: 'Key-Drop',
			reward: '1 Free Case',
			value: 0.5,
			description: 'Massive case opening platform with daily bonuses.',
			url: 'https://kd.link/?code=DIGIPULATION',
			code: 'DIGIPULATION',
			accentColor: '#2563eb',
			tag: 'POPULAR'
		},
		{
			id: 3,
			site: 'Howl.gg',
			reward: '1 Free Case',
			value: 0.5,
			description: 'Premium case battles and upgrades platform.',
			url: 'https://howl.gg/r/dpl',
			code: 'dpl',
			accentColor: '#7c3aed',
			tag: 'BATTLES'
		},
		{
			id: 4,
			site: 'Clash.gg',
			reward: '3 Free Cases',
			value: 1.5,
			description: 'Best value - triple the cases, triple the chances!',
			url: 'https://clash.gg/r/DIGIPULATION',
			code: 'DIGIPULATION',
			accentColor: '#059669',
			tag: 'BEST VALUE'
		},
		{
			id: 5,
			site: 'DatDrop',
			reward: 'Daily Free Case',
			value: 0.5,
			description: 'Come back every day for a new free case!',
			url: 'https://datdrop.com/p/digipulation',
			code: 'digipulation',
			accentColor: '#f59e0b',
			tag: 'DAILY'
		},
		{
			id: 6,
			site: 'Farmskins',
			reward: '$1 Free + Daily Cases',
			value: 1.0,
			description: 'Instant $1 balance plus daily free cases!',
			url: 'https://farmskins.com/ref-digipulation',
			code: 'digipulation',
			accentColor: '#ec4899',
			tag: 'FREE BALANCE'
		}
	];

	let showCopyToast = $state(false);

	onMount(() => {
		offers.forEach((_, index) => {
			setTimeout(
				() => {
					visibleSteps = [...visibleSteps, index];
				},
				120 * (index + 1)
			);
		});
	});

	function handleClaim(url) {
		window.open(url, '_blank');
	}

	async function copyCode(code) {
		try {
			await navigator.clipboard.writeText(code);
			showCopyToast = true;
			setTimeout(() => {
				showCopyToast = false;
			}, 2000);
		} catch (err) {
			console.error('Failed to copy:', err);
		}
	}

	// SEO Data
	const schemaOrg = {
		'@context': 'https://schema.org',
		'@graph': [
			{
				'@type': 'HowTo',
				name: 'How to Get Free CS2 Skins & Cases',
				description:
					'Follow this step-by-step path to claim 8+ free CS2 cases from trusted sites like Empire, Key-Drop, and Farmskins.',
				step: offers.map((offer, index) => ({
					'@type': 'HowToStep',
					position: index + 1,
					name: `Claim ${offer.reward} on ${offer.site}`,
					text: `Use code ${offer.code} on ${offer.site} to claim your reward.`,
					url: offer.url
				}))
			},
			{
				'@type': 'FAQPage',
				mainEntity: [
					{
						'@type': 'Question',
						name: 'Is this really free?',
						acceptedAnswer: {
							'@type': 'Answer',
							text: 'Yes! All cases are completely free. No deposits required.'
						}
					},
					{
						'@type': 'Question',
						name: 'How do I withdraw skins?',
						acceptedAnswer: {
							'@type': 'Answer',
							text: 'Each site lets you withdraw skins directly to your Steam account.'
						}
					},
					{
						'@type': 'Question',
						name: 'Can I claim from all sites?',
						acceptedAnswer: {
							'@type': 'Answer',
							text: 'Yes! Each site is independent. Complete all 6 for maximum rewards.'
						}
					}
				]
			}
		]
	};
</script>

<svelte:head>
	<title>How to Get Free CS2 Skins | Step-by-Step Guide 2025</title>
	<meta
		name="description"
		content="The ultimate guide to getting free CS2 skins and cases. Follow our verified path to claim 8+ free cases from trusted sites. No deposit required."
	/>
	<meta
		name="keywords"
		content="how to get free cs2 skins, free csgo skins guide, free cs2 cases no deposit, farmskins daily free case, csgoempire free case code"
	/>

	<!-- JSON-LD Structured Data -->
	{@html `<script type="application/ld+json">${JSON.stringify(schemaOrg)}</script>`}
</svelte:head>

<div class="min-h-screen bg-[#08090c]">
	<!-- Hero -->
	<section class="relative overflow-hidden pt-16 pb-12">
		<div
			class="absolute top-0 left-1/2 h-[300px] w-[600px] -translate-x-1/2 rounded-full bg-yellow-500/8 blur-[120px]"
		></div>

		<div class="relative z-10 container mx-auto max-w-4xl px-4">
			<div class="text-center">
				<div
					class="mb-6 inline-flex items-center gap-2 rounded-full border border-green-500/20 bg-green-500/10 px-3 py-1"
				>
					<div class="h-1.5 w-1.5 animate-pulse rounded-full bg-green-400"></div>
					<span class="text-xs font-medium text-green-400">100% FREE • NO DEPOSIT</span>
				</div>

				<h1 class="mb-3 text-4xl font-bold tracking-tight text-white md:text-5xl">
					Free Skins Path
				</h1>
				<p class="mx-auto mb-8 max-w-lg text-lg text-gray-500">
					Follow the path to claim <span class="text-white">8+ free cases</span> from 6 top sites
				</p>

				<div class="flex justify-center gap-6 text-sm">
					<div class="text-center">
						<div class="text-2xl font-bold text-yellow-400">8+</div>
						<div class="text-gray-600">Cases</div>
					</div>
					<div class="w-px bg-white/10"></div>
					<div class="text-center">
						<div class="text-2xl font-bold text-green-400">6</div>
						<div class="text-gray-600">Sites</div>
					</div>
					<div class="w-px bg-white/10"></div>
					<div class="text-center">
						<div class="text-2xl font-bold text-purple-400">$100s</div>
						<div class="text-gray-600">Potential</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Path Section -->
	<section class="py-8 pb-20">
		<div class="container mx-auto max-w-2xl px-4">
			<!-- Path wrapper -->
			<div class="relative">
				<!-- Vertical path line -->
				<div
					class="absolute top-0 bottom-0 left-6 w-px bg-gradient-to-b from-yellow-500/50 via-purple-500/50 to-pink-500/50"
				></div>

				<!-- Offers -->
				<div class="space-y-0">
					{#each offers as offer, index}
						<div
							class="relative transition-all duration-500 {visibleSteps.includes(index)
								? 'translate-x-0 opacity-100'
								: '-translate-x-4 opacity-0'}"
						>
							<!-- Connector dot -->
							<div
								class="absolute top-8 left-6 z-10 h-3 w-3 -translate-x-1/2 rounded-full border-2"
								style="background: {offer.accentColor}; border-color: {offer.accentColor}"
							></div>

							<!-- Horizontal connector line -->
							<div
								class="absolute top-[34px] left-6 h-px w-8"
								style="background: linear-gradient(to right, {offer.accentColor}, transparent)"
							></div>

							<!-- Card -->
							<div class="ml-16 pb-8">
								<div
									class="group relative overflow-hidden rounded-xl border border-white/5 bg-[#0d0e12] transition-all duration-300 hover:border-white/10 hover:bg-[#101117]"
								>
									<!-- Top accent -->
									<div class="h-0.5" style="background: {offer.accentColor}"></div>

									<div class="p-5">
										<!-- Header -->
										<div class="mb-3 flex items-start justify-between gap-3">
											<div class="flex items-center gap-3">
												<div
													class="flex h-9 w-9 items-center justify-center rounded-lg text-sm font-bold text-white"
													style="background: {offer.accentColor}"
												>
													{index + 1}
												</div>
												<div>
													<h3 class="font-semibold text-white">{offer.site}</h3>
													<p class="text-xs text-gray-500">{offer.description}</p>
												</div>
											</div>
											<span
												class="rounded px-2 py-0.5 text-[10px] font-bold tracking-wide text-white uppercase"
												style="background: {offer.accentColor}"
											>
												{offer.tag}
											</span>
										</div>

										<!-- Reward row -->
										<div class="mb-4 flex items-center justify-between rounded-lg bg-white/3 p-3">
											<div>
												<div class="text-[10px] tracking-wider text-gray-500 uppercase">Reward</div>
												<div class="text-lg font-bold" style="color: {offer.accentColor}">
													{offer.reward}
												</div>
											</div>
											<div class="text-right">
												<div class="text-[10px] tracking-wider text-gray-500 uppercase">Value</div>
												<div class="font-medium text-gray-300">~${offer.value.toFixed(2)}</div>
											</div>
										</div>

										<!-- Actions -->
										<div class="flex gap-2">
											<button
												onclick={() => copyCode(offer.code)}
												class="flex cursor-pointer items-center gap-2 rounded-lg border border-white/10 bg-white/5 px-3 py-2.5 text-xs font-medium text-gray-400 transition-all hover:border-white/20 hover:bg-white/10 hover:text-white"
											>
												<svg
													class="h-3.5 w-3.5"
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
												<span class="font-mono">{offer.code}</span>
											</button>

											<button
												onclick={() => handleClaim(offer.url)}
												class="flex flex-1 cursor-pointer items-center justify-center gap-2 rounded-lg px-4 py-2.5 text-xs font-bold text-white transition-all hover:brightness-110"
												style="background: {offer.accentColor}"
											>
												Claim Now
												<svg
													class="h-3.5 w-3.5"
													fill="none"
													stroke="currentColor"
													viewBox="0 0 24 24"
												>
													<path
														stroke-linecap="round"
														stroke-linejoin="round"
														stroke-width="2"
														d="M14 5l7 7m0 0l-7 7m7-7H3"
													/>
												</svg>
											</button>
										</div>
									</div>
								</div>
							</div>
						</div>
					{/each}

					<!-- End marker -->
					<div class="relative">
						<div
							class="absolute top-0 left-6 flex h-5 w-5 -translate-x-1/2 items-center justify-center rounded-full bg-gradient-to-br from-yellow-400 to-orange-500"
						>
							<svg class="h-3 w-3 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="3"
									d="M5 13l4 4L19 7"
								/>
							</svg>
						</div>
						<div class="ml-16 pt-1">
							<span class="text-xs font-medium text-yellow-400">Path Complete!</span>
							<p class="text-xs text-gray-600">
								Check back daily for fresh cases, bonuses & rewards
							</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Tips -->
	<section class="border-t border-white/5 py-12">
		<div class="container mx-auto max-w-2xl px-4">
			<h2 class="mb-4 flex items-center gap-2 text-lg font-bold text-white">
				<svg class="h-4 w-4 text-yellow-400" fill="currentColor" viewBox="0 0 20 20">
					<path
						fill-rule="evenodd"
						d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z"
						clip-rule="evenodd"
					/>
				</svg>
				Pro Tips
			</h2>
			<div class="grid gap-2 sm:grid-cols-2">
				<div class="flex items-center gap-2 rounded-lg bg-white/3 p-3 text-sm text-gray-400">
					<span class="text-green-400">✓</span>
					Complete all 6 sites for max cases
				</div>
				<div class="flex items-center gap-2 rounded-lg bg-white/3 p-3 text-sm text-gray-400">
					<span class="text-green-400">✓</span>
					Return daily for recurring rewards
				</div>
				<div class="flex items-center gap-2 rounded-lg bg-white/3 p-3 text-sm text-gray-400">
					<span class="text-green-400">✓</span>
					Check for extra daily bonuses
				</div>
				<div class="flex items-center gap-2 rounded-lg bg-white/3 p-3 text-sm text-gray-400">
					<span class="text-green-400">✓</span>
					Try case battles to multiply wins
				</div>
			</div>
		</div>
	</section>

	<!-- FAQ -->
	<section class="border-t border-white/5 py-12">
		<div class="container mx-auto max-w-2xl px-4">
			<h2 class="mb-4 text-lg font-bold text-white">FAQ</h2>
			<div class="space-y-2">
				<details class="group rounded-lg bg-white/3">
					<summary
						class="flex cursor-pointer items-center justify-between p-4 text-sm font-medium text-white"
					>
						Is this really free?
						<svg
							class="h-4 w-4 text-gray-500 transition-transform group-open:rotate-180"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M19 9l-7 7-7-7"
							/>
						</svg>
					</summary>
					<p class="px-4 pb-4 text-sm text-gray-500">
						Yes! All cases are completely free. No deposits required.
					</p>
				</details>
				<details class="group rounded-lg bg-white/3">
					<summary
						class="flex cursor-pointer items-center justify-between p-4 text-sm font-medium text-white"
					>
						How do I withdraw skins?
						<svg
							class="h-4 w-4 text-gray-500 transition-transform group-open:rotate-180"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M19 9l-7 7-7-7"
							/>
						</svg>
					</summary>
					<p class="px-4 pb-4 text-sm text-gray-500">
						Each site lets you withdraw skins directly to your Steam account.
					</p>
				</details>
				<details class="group rounded-lg bg-white/3">
					<summary
						class="flex cursor-pointer items-center justify-between p-4 text-sm font-medium text-white"
					>
						Can I claim from all sites?
						<svg
							class="h-4 w-4 text-gray-500 transition-transform group-open:rotate-180"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M19 9l-7 7-7-7"
							/>
						</svg>
					</summary>
					<p class="px-4 pb-4 text-sm text-gray-500">
						Yes! Each site is independent. Complete all 6 for maximum rewards.
					</p>
				</details>
			</div>
		</div>
	</section>
</div>

<!-- Toast -->
{#if showCopyToast}
	<div class="animate-slide-up fixed bottom-6 left-6 z-50">
		<div
			class="flex items-center gap-2 rounded-lg bg-green-500 px-4 py-2.5 text-sm font-medium text-white shadow-lg"
		>
			<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
			</svg>
			Code copied!
		</div>
	</div>
{/if}

<style>
	@keyframes slide-up {
		from {
			transform: translateY(16px);
			opacity: 0;
		}
		to {
			transform: translateY(0);
			opacity: 1;
		}
	}
	.animate-slide-up {
		animation: slide-up 0.2s ease-out;
	}

	details summary::-webkit-details-marker {
		display: none;
	}
	details summary {
		list-style: none;
	}
</style>
