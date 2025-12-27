<script>
	import { onMount } from 'svelte';

	// SEO Data
	const schemaOrg = {
		'@context': 'https://schema.org',
		'@graph': [
			{
				'@type': 'WebSite',
				'@id': 'https://cs2giveaways.com/#website',
				url: 'https://cs2giveaways.com/',
				name: 'CS2 Giveaways',
				description: 'Win free CS2 skins, open daily free cases, and get exclusive promo codes.',
				potentialAction: [
					{
						'@type': 'SearchAction',
						target: {
							'@type': 'EntryPoint',
							urlTemplate: 'https://cs2giveaways.com/search?q={search_term_string}'
						},
						'query-input': 'required name=search_term_string'
					}
				],
				inLanguage: 'en-US'
			},
			{
				'@type': 'Organization',
				'@id': 'https://cs2giveaways.com/#organization',
				name: 'CS2 Giveaways',
				url: 'https://cs2giveaways.com/',
				logo: {
					'@type': 'ImageObject',
					inLanguage: 'en-US',
					'@id': 'https://cs2giveaways.com/#/schema/logo/image/',
					url: 'https://cs2giveaways.com/logo.png',
					contentUrl: 'https://cs2giveaways.com/logo.png',
					width: 512,
					height: 512,
					caption: 'CS2 Giveaways'
				},
				sameAs: ['https://x.com/cs2giveawayspro']
			}
		]
	};

	let wheelRotation = $state(0);
	let isDragging = $state(false);
	let startX = $state(0);
	let startRotation = $state(0);
	let wheelElement = $state(null);

	// Spinning wheel state
	let spinWheelRotation = $state(0);
	let isSpinning = $state(false);
	let hasSpunThisWeek = $state(false);
	let wonPrize = $state(null);
	let showPrizeModal = $state(false);
	let timeUntilNextSpin = $state('');
	let lastSpinTime = $state(null);
	let unlockedOffers = $state([]);
	let showCopyToast = $state(false);

	// Spinning wheel prizes with affiliate links
	const spinPrizes = [
		{
			id: 1,
			name: 'CSGOEmpire',
			reward: '1 Free Case',
			color: '#dc2626',
			url: 'https://csgoempire.com/r/digipulation',
			code: 'digipulation'
		},
		{
			id: 2,
			name: 'Key-Drop',
			reward: '1 Free Case',
			color: '#2563eb',
			url: 'https://kd.link/?code=DIGIPULATION',
			code: 'DIGIPULATION'
		},
		{
			id: 3,
			name: 'Howl.gg',
			reward: '1 Free Case',
			color: '#7c3aed',
			url: 'https://howl.gg/r/dpl',
			code: 'dpl'
		},
		{
			id: 4,
			name: 'Clash.gg',
			reward: '3 Free Cases',
			color: '#059669',
			url: 'https://clash.gg/r/DIGIPULATION',
			code: 'DIGIPULATION'
		},
		{
			id: 5,
			name: 'DatDrop',
			reward: 'Daily Free Case',
			color: '#f59e0b',
			url: 'https://datdrop.com/p/digipulation',
			code: 'digipulation'
		},
		{
			id: 6,
			name: 'Farmskins',
			reward: 'Daily Free Cases',
			color: '#ec4899',
			url: 'https://farmskins.com/ref-digipulation',
			code: 'digipulation'
		}
	];

	const SPIN_STORAGE_KEY = 'cs2giveaways_last_spin';
	const UNLOCKED_STORAGE_KEY = 'cs2giveaways_unlocked';
	const LAST_PRIZE_STORAGE_KEY = 'cs2giveaways_last_prize';
	const ONE_DAY_MS = 24 * 60 * 60 * 1000;

	// Last won prize (persisted)
	let lastWonPrize = $state(null);

	// Check if user can spin
	function checkSpinEligibility() {
		if (typeof window === 'undefined') return;

		const stored = localStorage.getItem(SPIN_STORAGE_KEY);
		if (stored) {
			const lastSpin = parseInt(stored, 10);
			lastSpinTime = lastSpin;
			const now = Date.now();
			const timeSinceLastSpin = now - lastSpin;

			if (timeSinceLastSpin < ONE_DAY_MS) {
				hasSpunThisWeek = true;
				updateCountdown();
			} else {
				hasSpunThisWeek = false;
			}
		} else {
			hasSpunThisWeek = false;
		}
		const unlocked = JSON.parse(localStorage.getItem(UNLOCKED_STORAGE_KEY)) || [];
		unlockedOffers = unlocked;

		// Load last won prize
		const storedPrize = localStorage.getItem(LAST_PRIZE_STORAGE_KEY);
		if (storedPrize) {
			try {
				lastWonPrize = JSON.parse(storedPrize);
			} catch (e) {
				lastWonPrize = null;
			}
		}
	}

	// Update countdown timer
	function updateCountdown() {
		if (!lastSpinTime) return;

		const now = Date.now();
		const nextSpinTime = lastSpinTime + ONE_DAY_MS;
		const remaining = nextSpinTime - now;

		if (remaining <= 0) {
			hasSpunThisWeek = false;
			timeUntilNextSpin = '';
			return;
		}

		const days = Math.floor(remaining / (24 * 60 * 60 * 1000));
		const hours = Math.floor((remaining % (24 * 60 * 60 * 1000)) / (60 * 60 * 1000));
		const minutes = Math.floor((remaining % (60 * 60 * 1000)) / (60 * 1000));
		const seconds = Math.floor((remaining % (60 * 1000)) / 1000);

		if (days > 0) {
			timeUntilNextSpin = `${days}d ${hours}h ${minutes}m`;
		} else if (hours > 0) {
			timeUntilNextSpin = `${hours}h ${minutes}m ${seconds}s`;
		} else {
			timeUntilNextSpin = `${minutes}m ${seconds}s`;
		}
	}

	// Spin the wheel
	function spinTheWheel() {
		if (isSpinning || hasSpunThisWeek) return;

		isSpinning = true;

		// Random prize selection (weighted slightly towards Clash.gg for the 3 cases)
		const randomIndex = Math.floor(Math.random() * spinPrizes.length);
		const prize = spinPrizes[randomIndex];

		// Calculate rotation to land on the prize
		// Each segment is 90 degrees (360 / 4 prizes)
		const segmentAngle = 360 / spinPrizes.length;
		// We want the pointer at top (0 degrees) to point to the prize
		// Prize 0 is at 0-90, Prize 1 at 90-180, etc.
		const prizeAngle = randomIndex * segmentAngle + segmentAngle / 2;
		// Spin multiple times (8-12 full rotations) plus land on prize for realistic feel
		const fullRotations = (8 + Math.floor(Math.random() * 5)) * 360;
		const finalRotation = fullRotations + (360 - prizeAngle);

		spinWheelRotation = finalRotation;

		// After spin animation completes
		setTimeout(() => {
			isSpinning = false;
			wonPrize = prize;
			showPrizeModal = true;

			// Save spin time to localStorage
			const now = Date.now();
			localStorage.setItem(SPIN_STORAGE_KEY, now.toString());
			lastSpinTime = now;
			hasSpunThisWeek = true;
			updateCountdown();
			if (!unlockedOffers.includes(prize.name)) {
				unlockedOffers = [...unlockedOffers, prize.name];
				localStorage.setItem(UNLOCKED_STORAGE_KEY, JSON.stringify(unlockedOffers));
			}
			// Save last won prize
			lastWonPrize = prize;
			localStorage.setItem(LAST_PRIZE_STORAGE_KEY, JSON.stringify(prize));
		}, 7000); // 7 second spin animation
	}

	// Close prize modal
	function closePrizeModal() {
		showPrizeModal = false;
	}

	// Claim prize (redirect to affiliate link)
	function claimPrize() {
		if (wonPrize) {
			window.open(wonPrize.url, '_blank');
		}
		showPrizeModal = false;
	}

	// Copy prize code to clipboard
	async function copyPrizeCode() {
		if (!wonPrize) return;

		try {
			await navigator.clipboard.writeText(wonPrize.code);
			showCopyToast = true;
			setTimeout(() => {
				showCopyToast = false;
			}, 2000);
		} catch (err) {
			console.error('Failed to copy code:', err);
		}
	}

	// Copy last won prize code
	async function copyLastWonCode() {
		if (!lastWonPrize) return;

		try {
			await navigator.clipboard.writeText(lastWonPrize.code);
			showCopyToast = true;
			setTimeout(() => {
				showCopyToast = false;
			}, 2000);
		} catch (err) {
			console.error('Failed to copy code:', err);
		}
	}

	onMount(() => {
		checkSpinEligibility();

		// Update countdown every second
		const interval = setInterval(() => {
			if (hasSpunThisWeek) {
				updateCountdown();
			}
		}, 1000);

		return () => clearInterval(interval);
	});

	const exclusiveOffers = [
		{
			id: 1,
			name: 'CSGOEmpire',
			image: '/csgoempire-affiliate.png',
			claimUrl: 'https://csgoempire.com/r/digipulation'
		},
		{
			id: 2,
			name: 'Key-Drop',
			image: '/keydrop-promo-code.png',
			claimUrl: 'https://kd.link/?code=DIGIPULATION'
		},
		{
			id: 3,
			name: 'Clash.gg',
			image: '/clashgg-affiliate-offer.jpg',
			claimUrl: 'https://clash.gg/r/DIGIPULATION'
		}
	];

	const itemCount = exclusiveOffers.length;
	const anglePerItem = 360 / itemCount;

	function getClientX(e) {
		if (e.touches && e.touches.length > 0) {
			return e.touches[0].clientX;
		}
		if (e.changedTouches && e.changedTouches.length > 0) {
			return e.changedTouches[0].clientX;
		}
		return e.clientX;
	}

	function handleDragStart(e) {
		if (e.target.closest('a')) return;

		isDragging = true;
		startX = getClientX(e);
		startRotation = wheelRotation;

		if (e.target && e.target.setPointerCapture && e.pointerId !== undefined) {
			e.target.setPointerCapture(e.pointerId);
		}

		e.preventDefault();
	}

	function handleDragMove(e) {
		if (!isDragging) return;

		const currentX = getClientX(e);
		const delta = currentX - startX;
		wheelRotation = startRotation + delta * 0.5;

		e.preventDefault();
	}

	function handleDragEnd(e) {
		if (!isDragging) return;

		isDragging = false;

		if (e.target && e.target.releasePointerCapture && e.pointerId !== undefined) {
			try {
				e.target.releasePointerCapture(e.pointerId);
			} catch (err) {}
		}

		const snappedAngle = Math.round(wheelRotation / anglePerItem) * anglePerItem;
		wheelRotation = snappedAngle;
	}

	function rotateLeft() {
		wheelRotation -= anglePerItem;
	}

	function rotateRight() {
		wheelRotation += anglePerItem;
	}

	function goToSlide(index) {
		wheelRotation = -index * anglePerItem;
	}

	function getCurrentIndex() {
		const normalized = ((-wheelRotation % 360) + 360) % 360;
		return Math.round(normalized / anglePerItem) % itemCount;
	}
</script>

<svelte:head>
	<title>CS2 Giveaways | Daily Free Skins & Exclusive Promo Codes {new Date().getFullYear()}</title>
	<meta
		name="description"
		content="Win free CS2 skins daily! Spin the wheel for free cases, find the best promo codes for Empire, Key-Drop & more. 100% free, no deposit required."
	/>

	<!-- JSON-LD Structured Data -->
	{@html `<script type="application/ld+json">${JSON.stringify(schemaOrg)}</script>`}
</svelte:head>

<div class="container mx-auto max-w-6xl space-y-16 px-4 py-12">
	<div class="flex items-center gap-4">
		<div class="h-px flex-1 bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
		<span class="text-sm tracking-wider text-gray-600 uppercase">Daily Spin</span>
		<div class="h-px flex-1 bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
	</div>

	<section class="relative">
		<div class="mb-8 text-center">
			<h2 class="font-display mb-4 text-3xl font-bold text-white md:text-4xl">
				Spin to <span class="text-yellow-500">Win Free Cases</span>
			</h2>
			<p class="mx-auto max-w-2xl text-gray-400">
				Try your luck once per day! Spin the wheel and claim your exclusive free case offer from our
				partner sites.
			</p>
		</div>

		<div class="flex flex-col items-center">
			<!-- Wheel Container -->
			<div class="relative mb-8">
				<!-- Glow effect -->
				<div class="absolute inset-0 scale-110 rounded-full bg-yellow-500/20 blur-[60px]"></div>

				<!-- Pointer -->
				<div class="absolute top-0 left-1/2 z-20 -translate-x-1/2 -translate-y-2">
					<div
						class="h-0 w-0 border-t-[25px] border-r-[15px] border-l-[15px] border-t-yellow-500 border-r-transparent border-l-transparent drop-shadow-lg"
					></div>
				</div>

				<!-- Wheel -->
				<div
					class="relative h-[300px] w-[300px] md:h-[400px] md:w-[400px]"
					role="button"
					tabindex="0"
					onclick={!hasSpunThisWeek && !isSpinning ? spinTheWheel : null}
					onkeydown={(e) => {
						if (!hasSpunThisWeek && !isSpinning && (e.key === 'Enter' || e.key === ' ')) {
							e.preventDefault();
							spinTheWheel();
						}
					}}
				>
					<svg
						viewBox="0 0 400 400"
						class="h-full w-full drop-shadow-2xl"
						style="transform: rotate({spinWheelRotation}deg); transition: transform {isSpinning
							? '7s'
							: '0s'} cubic-bezier(0.15, 0.80, 0.10, 1.00); cursor: {!hasSpunThisWeek && !isSpinning
							? 'pointer'
							: 'default'};"
					>
						<!-- Outer ring -->
						<circle cx="200" cy="200" r="195" fill="none" stroke="#374151" stroke-width="10" />
						<circle cx="200" cy="200" r="190" fill="none" stroke="#1f2937" stroke-width="2" />

						<!-- Prize segments -->
						{#each spinPrizes as prize, index}
							{@const segmentAngle = 360 / spinPrizes.length}
							{@const startAngle = index * segmentAngle - 90}
							{@const endAngle = (index + 1) * segmentAngle - 90}
							{@const startRad = (startAngle * Math.PI) / 180}
							{@const endRad = (endAngle * Math.PI) / 180}
							{@const x1 = 200 + 180 * Math.cos(startRad)}
							{@const y1 = 200 + 180 * Math.sin(startRad)}
							{@const x2 = 200 + 180 * Math.cos(endRad)}
							{@const y2 = 200 + 180 * Math.sin(endRad)}
							{@const largeArc = segmentAngle > 180 ? 1 : 0}
							{@const midAngle = (startAngle + endAngle) / 2}
							{@const midRad = (midAngle * Math.PI) / 180}
							{@const textX = 200 + 120 * Math.cos(midRad)}
							{@const textY = 200 + 120 * Math.sin(midRad)}

							<path
								d="M 200 200 L {x1} {y1} A 180 180 0 {largeArc} 1 {x2} {y2} Z"
								fill={prize.color}
								stroke="#1f2937"
								stroke-width="2"
								class={`${!unlockedOffers.includes(prize.name) ? 'blur-sm' : ''}`}
							/>

							<!-- Prize text -->
							<g
								transform="rotate({midAngle + 90}, {textX}, {textY})"
								class={`${!unlockedOffers.includes(prize.name) ? 'blur-sm' : ''}`}
							>
								<text
									x={textX}
									y={textY - 15}
									text-anchor="middle"
									fill="white"
									font-size="14"
									font-weight="bold"
									class="drop-shadow"
								>
									{prize.name}
								</text>
								<text
									x={textX}
									y={textY + 5}
									text-anchor="middle"
									fill="rgba(255,255,255,0.9)"
									font-size="11"
								>
									{prize.reward}
								</text>
							</g>
						{/each}

						<!-- Center circle -->
						<circle cx="200" cy="200" r="40" fill="#1f2937" stroke="#374151" stroke-width="4" />
						<circle cx="200" cy="200" r="30" fill="#111827" stroke="#eab308" stroke-width="4" />
					</svg>
				</div>

				<!-- Decorative lights around wheel -->
				<div class="pointer-events-none absolute inset-0">
					{#each Array(12) as _, i}
						{@const angle = ((i * 30 - 90) * Math.PI) / 180}
						{@const x = 50 + 48 * Math.cos(angle)}
						{@const y = 50 + 48 * Math.sin(angle)}
						<div
							class="absolute h-2 w-2 animate-pulse rounded-full bg-yellow-400"
							style="left: {x}%; top: {y}%; animation-delay: {i * 100}ms;"
						></div>
					{/each}
				</div>
			</div>

			<!-- Spin Button -->
			{#if hasSpunThisWeek}
				<div class="text-center">
					<div
						class="inline-flex items-center gap-3 rounded-full border border-white/10 bg-gray-800/50 px-8 py-4"
					>
						<svg
							class="h-6 w-6 text-gray-500"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
							/>
						</svg>
						<div class="text-left">
							<p class="text-sm text-gray-400">Next spin available in</p>
							<p class="text-lg font-bold text-white">{timeUntilNextSpin}</p>
						</div>
					</div>
					<p class="mt-4 text-sm text-gray-500">You can spin once per day</p>

					<!-- Last Won Prize Section -->
					{#if lastWonPrize}
						<div class="mx-auto mt-6 w-full max-w-md">
							<div
								class="rounded-xl border border-white/10 bg-gradient-to-br from-[#1a1d24] to-[#12141a] p-4"
							>
								<p class="mb-2 text-xs tracking-wider text-gray-500 uppercase">Your Prize</p>
								<div class="mb-3 flex items-center gap-3">
									<div
										class="flex h-10 w-10 items-center justify-center rounded-lg"
										style="background: {lastWonPrize.color}"
									>
										<svg
											class="h-5 w-5 text-white"
											fill="none"
											stroke="currentColor"
											viewBox="0 0 24 24"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7"
											/>
										</svg>
									</div>
									<div class="flex-1 text-left">
										<p class="font-semibold text-white">{lastWonPrize.name}</p>
										<p class="text-sm text-yellow-500">{lastWonPrize.reward}</p>
									</div>
								</div>

								<!-- Code with Copy -->
								<button
									onclick={copyLastWonCode}
									class="group mb-3 flex w-full items-center justify-between gap-3 rounded-lg border border-white/10 bg-white/5 px-4 py-2.5 transition-all hover:border-yellow-500/50 hover:bg-white/10"
								>
									<span class="font-mono font-bold tracking-wider text-yellow-500"
										>{lastWonPrize.code}</span
									>
									<div
										class="flex items-center gap-1.5 text-gray-400 transition-colors group-hover:text-yellow-500"
									>
										<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"
											/>
										</svg>
										<span class="text-xs font-medium">Copy</span>
									</div>
								</button>

								<!-- Claim Button -->
								<a
									href={lastWonPrize.url}
									target="_blank"
									rel="noopener noreferrer"
									class="flex w-full items-center justify-center gap-2 rounded-lg px-4 py-2.5 text-sm font-bold text-white transition-all hover:scale-105"
									style="background: {lastWonPrize.color}"
								>
									Claim at {lastWonPrize.name}
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
					{/if}
				</div>
			{:else}
				<p class="mt-4 text-sm text-gray-500">Click the wheel to spin! One free spin per day!</p>
			{/if}
		</div>

		<!-- Key-Drop Banner -->
		<div class="mt-12">
			<!-- Mobile Banner (static design) -->
			<a
				href="https://kd.link/?code=DIGIPULATION"
				target="_blank"
				rel="noopener noreferrer"
				aria-label="Key-Drop Promo - Get 20% bonus and 1 USD with every top-up, plus a free daily case"
				class="block overflow-hidden rounded-xl border border-white/10 bg-gradient-to-r from-[#0a0a1a] via-[#1a1a3a] to-[#0a0a1a] p-4 transition-colors duration-300 hover:border-yellow-500/30 md:hidden"
			>
				<div class="flex items-center justify-between gap-4">
					<div class="flex-1">
						<div class="mb-2 flex items-center gap-2">
							<span class="text-lg font-bold text-yellow-400">🎁 keydrop</span>
						</div>
						<div class="mb-1 text-xl font-bold text-white">20% + 1 USD</div>
						<div class="text-sm text-gray-300">BONUS WITH EVERY TOP-UP</div>
						<div
							class="mt-3 inline-flex items-center gap-2 rounded-lg bg-green-500 px-4 py-2 text-sm font-bold text-white transition-colors hover:bg-green-400"
						>
							<span>+</span> FREE DAILY CASE
						</div>
					</div>
					<div class="flex-shrink-0">
						<img
							src="/keydrop-promo-code.png"
							alt="Key-Drop"
							class="h-20 w-20 rounded-lg object-cover opacity-80"
						/>
					</div>
				</div>
			</a>

			<!-- Desktop Banner (iframe embed with full height) -->
			<a
				href="https://kd.link/?code=DIGIPULATION"
				target="_blank"
				rel="noopener noreferrer"
				aria-label="Key-Drop Promo - Get bonus with code DIGIPULATION"
				class="keydrop-banner-container hidden overflow-hidden rounded-xl border border-white/10 transition-colors duration-300 hover:border-yellow-500/30 md:block"
			>
				<iframe
					src="https://kd.media/banner?code=DIGIPULATION"
					title="Key-Drop Promo Banner"
					class="keydrop-banner-iframe"
					scrolling="no"
					style="pointer-events: none;"
				></iframe>
			</a>
		</div>
	</section>

	<section class="relative">
		<h1 class="mb-4 flex justify-center">
			<svg
				viewBox="0 0 450 70"
				class="h-auto w-full max-w-lg"
				xmlns="http://www.w3.org/2000/svg"
				aria-label="CS2 Giveaways Exclusives"
			>
				<defs>
					<linearGradient id="goldGradient" x1="0%" y1="0%" x2="100%" y2="0%">
						<stop offset="0%" style="stop-color:#f59e0b;stop-opacity:1" />
						<stop offset="50%" style="stop-color:#eab308;stop-opacity:1" />
						<stop offset="100%" style="stop-color:#f59e0b;stop-opacity:1" />
					</linearGradient>
					<filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
						<feGaussianBlur stdDeviation="3" result="coloredBlur" />
						<feMerge>
							<feMergeNode in="coloredBlur" />
							<feMergeNode in="SourceGraphic" />
						</feMerge>
					</filter>
				</defs>

				<line
					x1="0"
					y1="35"
					x2="70"
					y2="35"
					stroke="url(#goldGradient)"
					stroke-width="2"
					opacity="0.5"
				/>
				<line
					x1="380"
					y1="35"
					x2="450"
					y2="35"
					stroke="url(#goldGradient)"
					stroke-width="2"
					opacity="0.5"
				/>

				<text x="80" y="44" fill="#eab308" font-size="24" filter="url(#glow)">★</text>
				<text x="360" y="44" fill="#eab308" font-size="24" filter="url(#glow)">★</text>

				<text
					x="225"
					y="32"
					text-anchor="middle"
					fill="url(#goldGradient)"
					font-family="Chakra Petch, sans-serif"
					font-weight="700"
					font-size="28"
					letter-spacing="2"
					filter="url(#glow)"
				>
					CS2GIVEAWAYS
				</text>
				<text
					x="225"
					y="58"
					text-anchor="middle"
					fill="#ffffff"
					font-family="Chakra Petch, sans-serif"
					font-weight="600"
					font-size="18"
					letter-spacing="5"
					opacity="0.9"
				>
					EXCLUSIVES
				</text>
			</svg>
		</h1>

		<div class="relative flex flex-col items-center">
			<div class="pointer-events-none absolute inset-0 flex items-center justify-center">
				<div class="h-[600px] w-[600px] rounded-full bg-yellow-500/5 blur-[100px]"></div>
				<div class="absolute h-[400px] w-[400px] rounded-full bg-purple-500/10 blur-[80px]"></div>
			</div>

			<button
				onclick={rotateLeft}
				class="absolute top-1/2 left-4 z-20 flex h-12 w-12 -translate-y-1/2 items-center justify-center rounded-full border border-white/10 bg-black/50 text-gray-400 backdrop-blur-sm transition-all duration-300 hover:border-yellow-500/50 hover:bg-yellow-500/10 hover:text-yellow-500 active:scale-95 md:left-8"
				aria-label="Previous offer"
			>
				<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M15 19l-7-7 7-7"
					/>
				</svg>
			</button>

			<button
				onclick={rotateRight}
				class="absolute top-1/2 right-4 z-20 flex h-12 w-12 -translate-y-1/2 items-center justify-center rounded-full border border-white/10 bg-black/50 text-gray-400 backdrop-blur-sm transition-all duration-300 hover:border-yellow-500/50 hover:bg-yellow-500/10 hover:text-yellow-500 active:scale-95 md:right-8"
				aria-label="Next offer"
			>
				<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
				</svg>
			</button>

			<!-- Desktop 3D Carousel -->
			<div
				class="relative hidden h-[550px] w-full max-w-5xl cursor-grab touch-none active:cursor-grabbing md:block md:h-[620px]"
				style="perspective: 1200px;"
				bind:this={wheelElement}
				onpointerdown={handleDragStart}
				onpointermove={handleDragMove}
				onpointerup={handleDragEnd}
				onpointercancel={handleDragEnd}
				role="region"
				aria-label="Exclusive offers carousel"
			>
				<div
					class="relative h-full w-full select-none"
					style="transform-style: preserve-3d; transform: rotateY({wheelRotation}deg); transition: transform {isDragging
						? '0ms'
						: '500ms'} ease-out;"
				>
					{#each exclusiveOffers as offer, index}
						{@const angle = index * anglePerItem}
						{@const translateZ = 220}
						<div
							class="pointer-events-auto absolute top-1/2 left-1/2 w-[300px] -translate-x-1/2 -translate-y-1/2"
							style="transform: rotateY({angle}deg) translateZ({translateZ}px);"
						>
							<a
								href={offer.claimUrl}
								target="_blank"
								rel="noopener noreferrer"
								class="group relative block overflow-hidden rounded-2xl shadow-2xl transition-all duration-500 hover:scale-105"
							>
								<!-- Outer glow on hover -->
								<div
									class="absolute -inset-1 rounded-2xl bg-gradient-to-r from-yellow-500 via-yellow-400 to-yellow-500 opacity-0 blur-md transition-opacity duration-500 group-hover:opacity-75"
								></div>

								<!-- Card container -->
								<div
									class="relative overflow-hidden rounded-2xl border-2 border-white/10 transition-colors duration-300 group-hover:border-yellow-500/50"
								>
									<!-- Image -->
									<div class="relative aspect-[1024/1269] overflow-hidden">
										<img
											src={offer.image}
											alt="{offer.name} Exclusive Offer"
											class="h-full w-full object-cover transition-transform duration-700 group-hover:scale-110"
											draggable="false"
										/>

										<!-- Shine effect on hover -->
										<div
											class="absolute inset-0 translate-x-[-100%] bg-gradient-to-tr from-transparent via-white/20 to-transparent transition-transform duration-1000 ease-out group-hover:translate-x-[100%]"
										></div>

										<!-- Subtle vignette -->
										<div
											class="pointer-events-none absolute inset-0 shadow-[inset_0_0_50px_rgba(0,0,0,0.3)]"
										></div>
									</div>

									<!-- Bottom highlight bar -->
									<div
										class="absolute right-0 bottom-0 left-0 h-1 bg-gradient-to-r from-transparent via-yellow-500 to-transparent opacity-0 transition-opacity duration-300 group-hover:opacity-100"
									></div>
								</div>
							</a>
						</div>
					{/each}
				</div>
			</div>

			<!-- Mobile Swipeable Cards -->
			<div
				class="relative w-full touch-pan-y overflow-hidden md:hidden"
				onpointerdown={handleDragStart}
				onpointermove={handleDragMove}
				onpointerup={handleDragEnd}
				onpointercancel={handleDragEnd}
			>
				<div
					class="flex gap-4 px-4 py-4 select-none"
					style="transform: translateX(calc(50% - 150px - {getCurrentIndex() *
						308}px)); transition: transform {isDragging ? '0ms' : '300ms'} ease-out;"
				>
					{#each exclusiveOffers as offer, index}
						{@const isActive = getCurrentIndex() === index}
						<a
							href={offer.claimUrl}
							target="_blank"
							rel="noopener noreferrer"
							class="group relative block w-[300px] flex-shrink-0"
							style="opacity: {isActive ? 1 : 0.5}; transform: scale({isActive
								? 1
								: 0.9}); transition: opacity 300ms, transform 300ms;"
						>
							<!-- Outer glow -->
							<div
								class="absolute -inset-1 rounded-2xl bg-gradient-to-r from-yellow-500 via-yellow-400 to-yellow-500 opacity-0 {isActive
									? 'group-hover:opacity-75'
									: ''} blur-md transition-opacity duration-500"
							></div>

							<!-- Card -->
							<div
								class="relative overflow-hidden rounded-2xl border-2 {isActive
									? 'border-yellow-500/30'
									: 'border-white/10'} transition-colors duration-300"
							>
								<div class="relative aspect-[1024/1269] overflow-hidden">
									<img
										src={offer.image}
										alt="{offer.name} Exclusive Offer"
										class="h-full w-full object-cover"
										draggable="false"
									/>

									<!-- Subtle vignette -->
									<div
										class="pointer-events-none absolute inset-0 shadow-[inset_0_0_50px_rgba(0,0,0,0.3)]"
									></div>
								</div>

								<!-- Bottom highlight bar for active -->
								{#if isActive}
									<div
										class="absolute right-0 bottom-0 left-0 h-1 bg-gradient-to-r from-transparent via-yellow-500 to-transparent"
									></div>
								{/if}
							</div>
						</a>
					{/each}
				</div>
			</div>

			<div class="mt-6 flex gap-3">
				{#each exclusiveOffers as _, index}
					<button
						onclick={() => goToSlide(index)}
						class="h-3 rounded-full transition-all duration-300 {getCurrentIndex() === index
							? 'w-8 bg-yellow-500'
							: 'w-3 bg-white/20 hover:bg-white/40'}"
						aria-label="Go to offer {index + 1}"
					></button>
				{/each}
			</div>
		</div>
	</section>

	<div class="flex items-center gap-4">
		<div class="h-px flex-1 bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
		<span class="text-sm tracking-wider text-gray-600 uppercase">How It Works</span>
		<div class="h-px flex-1 bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
	</div>

	<section class="relative">
		<div class="mb-12 text-center">
			<h2 class="font-display mb-4 text-3xl font-bold text-white md:text-4xl">
				Win <span class="text-yellow-500">CS2 Skins</span> in 3 Easy Steps
			</h2>
			<p class="mx-auto max-w-2xl text-gray-400">
				Join thousands of players who have already won free CS2 skins through our curated giveaways
				and exclusive partner offers.
			</p>
		</div>

		<div class="grid gap-6 md:grid-cols-3 lg:gap-8">
			<!-- Step 1 -->
			<div class="group relative">
				<div
					class="absolute -inset-0.5 rounded-2xl bg-gradient-to-r from-yellow-500 to-yellow-600 opacity-0 blur transition-opacity duration-300 group-hover:opacity-20"
				></div>
				<div
					class="relative h-full rounded-2xl border border-white/10 bg-[#1a1d24] p-6 transition-colors duration-300 hover:border-yellow-500/30"
				>
					<div
						class="mb-5 flex h-14 w-14 items-center justify-center rounded-xl border border-yellow-500/20 bg-yellow-500/10"
					>
						<span class="text-2xl font-bold text-yellow-500">1</span>
					</div>
					<h3 class="mb-3 text-xl font-semibold text-white">Browse Offers</h3>
					<p class="text-sm leading-relaxed text-gray-400">
						Explore our exclusive partner offers above. Each offer gives you free bonuses, cases, or
						coins when you sign up using our codes.
					</p>
					<div class="mt-4 border-t border-white/5 pt-4">
						<div class="flex items-center gap-2 text-xs text-gray-500">
							<svg
								class="h-4 w-4 text-green-400"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
								/>
							</svg>
							<span>Verified & trusted partners</span>
						</div>
					</div>
				</div>
			</div>

			<!-- Step 2 -->
			<div class="group relative">
				<div
					class="absolute -inset-0.5 rounded-2xl bg-gradient-to-r from-yellow-500 to-yellow-600 opacity-0 blur transition-opacity duration-300 group-hover:opacity-20"
				></div>
				<div
					class="relative h-full rounded-2xl border border-white/10 bg-[#1a1d24] p-6 transition-colors duration-300 hover:border-yellow-500/30"
				>
					<div
						class="mb-5 flex h-14 w-14 items-center justify-center rounded-xl border border-yellow-500/20 bg-yellow-500/10"
					>
						<span class="text-2xl font-bold text-yellow-500">2</span>
					</div>
					<h3 class="mb-3 text-xl font-semibold text-white">Claim Your Bonus</h3>
					<p class="text-sm leading-relaxed text-gray-400">
						Click on any offer to visit the site. Sign up or log in and use our promo code to
						instantly receive your free bonus rewards.
					</p>
					<div class="mt-4 border-t border-white/5 pt-4">
						<div class="flex items-center gap-2 text-xs text-gray-500">
							<svg
								class="h-4 w-4 text-green-400"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
								/>
							</svg>
							<span>Instant bonus activation</span>
						</div>
					</div>
				</div>
			</div>

			<!-- Step 3 -->
			<div class="group relative">
				<div
					class="absolute -inset-0.5 rounded-2xl bg-gradient-to-r from-yellow-500 to-yellow-600 opacity-0 blur transition-opacity duration-300 group-hover:opacity-20"
				></div>
				<div
					class="relative h-full rounded-2xl border border-white/10 bg-[#1a1d24] p-6 transition-colors duration-300 hover:border-yellow-500/30"
				>
					<div
						class="mb-5 flex h-14 w-14 items-center justify-center rounded-xl border border-yellow-500/20 bg-yellow-500/10"
					>
						<span class="text-2xl font-bold text-yellow-500">3</span>
					</div>
					<h3 class="mb-3 text-xl font-semibold text-white">Win & Withdraw</h3>
					<p class="text-sm leading-relaxed text-gray-400">
						Use your bonuses to play games, open cases, or enter giveaways. Win CS2 skins and
						withdraw them directly to your Steam account.
					</p>
					<div class="mt-4 border-t border-white/5 pt-4">
						<div class="flex items-center gap-2 text-xs text-gray-500">
							<svg
								class="h-4 w-4 text-green-400"
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
							<span>Direct Steam withdrawals</span>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	{#if showPrizeModal && wonPrize}
		<div class="fixed inset-0 z-50 flex items-center justify-center p-4">
			<!-- Backdrop -->
			<button
				type="button"
				class="absolute inset-0 cursor-default bg-black/80 backdrop-blur-sm"
				onclick={closePrizeModal}
				aria-label="Close modal"
			></button>

			<!-- Modal -->
			<div
				class="animate-bounce-in relative w-full max-w-sm transform overflow-hidden rounded-3xl border border-white/10 shadow-2xl"
				style="background: linear-gradient(145deg, {wonPrize.color}15, #12141a 40%, #1a1d24);"
			>
				<!-- Close Button -->
				<button
					onclick={closePrizeModal}
					class="absolute top-4 right-4 z-20 flex h-8 w-8 items-center justify-center rounded-full bg-white/10 text-gray-400 transition-all hover:bg-white/20 hover:text-white"
					aria-label="Close"
				>
					<svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M6 18L18 6M6 6l12 12"
						/>
					</svg>
				</button>

				<!-- Glow effect at top -->
				<div
					class="absolute top-0 left-1/2 h-48 w-48 -translate-x-1/2 rounded-full opacity-40 blur-[80px]"
					style="background: {wonPrize.color}"
				></div>

				<!-- Confetti effect -->
				<div class="pointer-events-none absolute inset-0 overflow-hidden">
					{#each Array(20) as _, i}
						<div
							class="animate-confetti absolute h-2 w-2 rounded-full"
							style="
								left: {Math.random() * 100}%;
								background: {['#eab308', '#22c55e', '#3b82f6', '#ef4444', '#a855f7'][i % 5]};
								animation-delay: {Math.random() * 0.5}s;
							"
						></div>
					{/each}
				</div>

				<!-- Content -->
				<div class="relative z-10 p-6 pt-8">
					<!-- Prize Icon -->
					<div class="mb-4 flex justify-center">
						<div
							class="flex h-16 w-16 items-center justify-center rounded-2xl shadow-lg"
							style="background: {wonPrize.color}; box-shadow: 0 8px 32px {wonPrize.color}60;"
						>
							<svg class="h-8 w-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7"
								/>
							</svg>
						</div>
					</div>

					<!-- Title -->
					<div class="mb-5 text-center">
						<h3 class="mb-1 text-xl font-bold text-white">🎉 You Won!</h3>
						<p class="text-2xl font-bold" style="color: {wonPrize.color}">{wonPrize.reward}</p>
						<p class="mt-1 text-sm text-gray-400">at {wonPrize.name}</p>
					</div>

					<!-- Promo Code Box -->
					<div class="mb-4">
						<p class="mb-2 text-center text-xs tracking-wider text-gray-500 uppercase">
							Your Promo Code
						</p>
						<button
							onclick={copyPrizeCode}
							class="group flex w-full items-center justify-between gap-3 rounded-xl border-2 border-dashed px-4 py-3 transition-all hover:border-solid"
							style="border-color: {wonPrize.color}50; background: {wonPrize.color}10;"
						>
							<span
								class="font-mono text-lg font-bold tracking-widest"
								style="color: {wonPrize.color}">{wonPrize.code}</span
							>
							<div
								class="flex items-center gap-1.5 text-gray-400 transition-colors group-hover:text-white"
							>
								<svg class="h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"
									/>
								</svg>
								<span class="text-xs font-semibold">COPY</span>
							</div>
						</button>
					</div>

					<!-- Action Button -->
					<button
						onclick={claimPrize}
						class="flex w-full items-center justify-center gap-2 rounded-xl px-6 py-4 font-bold text-white transition-all hover:scale-[1.02] hover:shadow-xl active:scale-[0.98]"
						style="background: linear-gradient(135deg, {wonPrize.color}, {wonPrize.color}cc); box-shadow: 0 4px 20px {wonPrize.color}40;"
					>
						<span>Claim at {wonPrize.name}</span>
						<svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"
							/>
						</svg>
					</button>

					<p class="mt-3 text-center text-xs text-gray-500">
						Copy the code, then claim your prize!
					</p>
				</div>
			</div>
		</div>
	{/if}

	<!-- Copy Toast Notification -->
	{#if showCopyToast}
		<div class="animate-slide-up fixed bottom-6 left-6 z-50">
			<div
				class="flex items-center gap-3 rounded-xl bg-green-500 px-5 py-3 text-white shadow-lg shadow-green-500/30"
			>
				<svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M5 13l4 4L19 7"
					/>
				</svg>
				<span class="font-medium">Code copied!</span>
			</div>
		</div>
	{/if}
</div>

<style>
	:global {
		@keyframes confetti {
			0% {
				transform: translateY(-10px) rotate(0deg);
				opacity: 1;
			}
			100% {
				transform: translateY(400px) rotate(720deg);
				opacity: 0;
			}
		}
	}

	.animate-confetti {
		animation: confetti 3s ease-out forwards;
	}

	:global {
		@keyframes bounce-in {
			0% {
				transform: scale(0.5);
				opacity: 0;
			}
			50% {
				transform: scale(1.05);
			}
			100% {
				transform: scale(1);
				opacity: 1;
			}
		}
	}

	.animate-bounce-in {
		animation: bounce-in 0.5s ease-out forwards;
	}

	:global {
		@keyframes slide-up {
			0% {
				transform: translateY(20px);
				opacity: 0;
			}
			100% {
				transform: translateY(0);
				opacity: 1;
			}
		}
	}

	.animate-slide-up {
		animation: slide-up 0.3s ease-out forwards;
	}

	/* Key-Drop Banner Container */
	.keydrop-banner-container {
		background: linear-gradient(to right, #0a0a1a, #0f1029, #0a0a1a);
		height: 126px;
		position: relative;
		display: block;
	}

	.keydrop-banner-iframe {
		width: 100%;
		height: 800px;
		border: none;
		display: block;
		margin-top: -456px;
	}

	@media (min-width: 768px) {
		.keydrop-banner-container {
			height: 147px;
		}
		.keydrop-banner-iframe {
			height: 850px;
			margin-top: -475px;
		}
	}

	@media (min-width: 1024px) {
		.keydrop-banner-container {
			height: 168px;
		}
		.keydrop-banner-iframe {
			height: 900px;
			margin-top: -494px;
		}
	}
</style>
