<script>
	import { onMount } from 'svelte';
	
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
	
	// Spinning wheel prizes with affiliate links
	const spinPrizes = [
		{
			id: 1,
			name: 'CSGOEmpire',
			reward: '1 Free Case',
			color: '#dc2626', // red
			url: 'https://csgoempire.com/r/digipulation'
		},
		{
			id: 2,
			name: 'Key-Drop',
			reward: '1 Free Case',
			color: '#2563eb', // blue
			url: 'https://kd.link/?code=DIGIPULATION'
		},
		{
			id: 3,
			name: 'Howl.gg',
			reward: '1 Free Case',
			color: '#7c3aed', // purple
			url: 'https://howl.gg/r/dpl'
		},
		{
			id: 4,
			name: 'Clash.gg',
			reward: '3 Free Cases',
			color: '#059669', // green
			url: 'https://clash.gg/r/DIGIPULATION'
		}
	];
	
	const SPIN_STORAGE_KEY = 'cs2giveaways_last_spin';
	const UNLOCKED_STORAGE_KEY = 'cs2giveaways_unlocked';
	const ONE_DAY_MS = 24 * 60 * 60 * 1000;
	
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
		// Spin multiple times (5-8 full rotations) plus land on prize
		const fullRotations = (5 + Math.floor(Math.random() * 3)) * 360;
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
		}, 5000); // 5 second spin animation
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
		const normalized = (((-wheelRotation % 360) + 360) % 360);
		return Math.round(normalized / anglePerItem) % itemCount;
	}
</script>

<svelte:head>
	<script type="application/ld+json">
	{
	  "@context": "https://schema.org",
	  "@graph": [
		{
		  "@type": "WebSite",
		  "@id": "https://cs2giveaways.com/#website",
		  "url": "https://cs2giveaways.com/",
		  "name": "CS2 Giveaways",
		  "description": "Win free CS2 skins, open daily free cases, and get exclusive promo codes.",
		  "inLanguage": "en-US"
		},
		{
		  "@type": "Organization",
		  "@id": "https://cs2giveaways.com/#organization",
		  "name": "CS2 Giveaways",
		  "url": "https://cs2giveaways.com/",
		  "logo": {
			"@type": "ImageObject",
			"inLanguage": "en-US",
			"@id": "https://cs2giveaways.com/#/schema/logo/image/",
			"url": "https://cs2giveaways.com/logo.png",
			"contentUrl": "https://cs2giveaways.com/logo.png",
			"width": 512,
			"height": 512,
			"caption": "CS2 Giveaways"
		  },
		  "image": {
			"@id": "https://cs2giveaways.com/#/schema/logo/image/"
		  },
		  "sameAs": [
			"https://x.com/cs2giveawayspro"
		  ]
		},
		{
		  "@type": "WebPage",
		  "@id": "https://cs2giveaways.com/#webpage",
		  "url": "https://cs2giveaways.com/",
		  "name": "CS2 Giveaways | Win Free Skins, Cases & Promo Codes Daily",
		  "isPartOf": {
			"@id": "https://cs2giveaways.com/#website"
		  },
		  "about": {
			"@id": "https://cs2giveaways.com/#organization"
		  },
		  "description": "Join CS2 Giveaways to win free CS2 skins, open daily free cases, and get exclusive promo codes for top CS2 sites. Spin the wheel and claim your rewards!",
		  "inLanguage": "en-US"
		}
	  ]
	}
	</script>
</svelte:head>

<div class="container mx-auto max-w-6xl px-4 py-12 space-y-16">

	<div class="flex items-center gap-4">
		<div class="flex-1 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
		<span class="text-gray-600 text-sm uppercase tracking-wider">Daily Spin</span>
		<div class="flex-1 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
	</div>

	<section class="relative">
		<div class="text-center mb-8">
			<h2 class="font-display font-bold text-3xl md:text-4xl text-white mb-4">
				Spin to <span class="text-yellow-500">Win Free Cases</span>
			</h2>
			<p class="text-gray-400 max-w-2xl mx-auto">
				Try your luck once per day! Spin the wheel and claim your exclusive free case offer from our partner sites.
			</p>
		</div>

		<div class="flex flex-col items-center">
			<!-- Wheel Container -->
			<div class="relative mb-8">
				<!-- Glow effect -->
				<div class="absolute inset-0 bg-yellow-500/20 rounded-full blur-[60px] scale-110"></div>
				
				<!-- Pointer -->
				<div class="absolute top-0 left-1/2 -translate-x-1/2 -translate-y-2 z-20">
					<div class="w-0 h-0 border-l-[15px] border-r-[15px] border-t-[25px] border-l-transparent border-r-transparent border-t-yellow-500 drop-shadow-lg"></div>
				</div>
				
				<!-- Wheel -->
				<div class="relative w-[300px] h-[300px] md:w-[400px] md:h-[400px]" role="button" tabindex="0" onclick={!hasSpunThisWeek && !isSpinning ? spinTheWheel : null} onkeydown={(e) => { if (!hasSpunThisWeek && !isSpinning && (e.key === 'Enter' || e.key === ' ')) { e.preventDefault(); spinTheWheel(); } }}>
					<svg
						viewBox="0 0 400 400"
						class="w-full h-full drop-shadow-2xl"
						style="transform: rotate({spinWheelRotation}deg); transition: transform {isSpinning ? '5s' : '0s'} cubic-bezier(0.17, 0.67, 0.12, 0.99); cursor: {!hasSpunThisWeek && !isSpinning ? 'pointer' : 'default'};"
					>
						<!-- Outer ring -->
						<circle cx="200" cy="200" r="195" fill="none" stroke="#374151" stroke-width="10"/>
						<circle cx="200" cy="200" r="190" fill="none" stroke="#1f2937" stroke-width="2"/>
						
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
							<g transform="rotate({midAngle + 90}, {textX}, {textY})" class={`${!unlockedOffers.includes(prize.name) ? 'blur-sm' : ''}`}>
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
						<circle cx="200" cy="200" r="40" fill="#1f2937" stroke="#374151" stroke-width="4"/>
						<circle cx="200" cy="200" r="30" fill="#111827" stroke="#eab308" stroke-width="4"/>
					</svg>
				</div>
				
				<!-- Decorative lights around wheel -->
				<div class="absolute inset-0 pointer-events-none">
					{#each Array(12) as _, i}
						{@const angle = (i * 30 - 90) * Math.PI / 180}
						{@const x = 50 + 48 * Math.cos(angle)}
						{@const y = 50 + 48 * Math.sin(angle)}
						<div
							class="absolute w-2 h-2 rounded-full bg-yellow-400 animate-pulse"
							style="left: {x}%; top: {y}%; animation-delay: {i * 100}ms;"
						></div>
					{/each}
				</div>
			</div>
			
			<!-- Spin Button -->
			{#if hasSpunThisWeek}
				<div class="text-center">
					<div class="inline-flex items-center gap-3 px-8 py-4 bg-gray-800/50 border border-white/10 rounded-full">
						<svg class="w-6 h-6 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"/>
						</svg>
						<div class="text-left">
							<p class="text-gray-400 text-sm">Next spin available in</p>
							<p class="text-white font-bold text-lg">{timeUntilNextSpin}</p>
						</div>
					</div>
					<p class="text-gray-500 text-sm mt-4">You can spin once per day</p>
				</div>
			{:else}
				<p class="text-gray-500 text-sm mt-4">Click the wheel to spin! One free spin per day!</p>
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
					class="md:hidden block overflow-hidden rounded-xl border border-white/10 hover:border-yellow-500/30 transition-colors duration-300 bg-gradient-to-r from-[#0a0a1a] via-[#1a1a3a] to-[#0a0a1a] p-4"
				>
					<div class="flex items-center justify-between gap-4">
						<div class="flex-1">
							<div class="flex items-center gap-2 mb-2">
								<span class="text-yellow-400 font-bold text-lg">🎁 keydrop</span>
							</div>
							<div class="text-white font-bold text-xl mb-1">20% + 1 USD</div>
							<div class="text-gray-300 text-sm">BONUS WITH EVERY TOP-UP</div>
							<div class="mt-3 inline-flex items-center gap-2 bg-green-500 hover:bg-green-400 text-white text-sm font-bold px-4 py-2 rounded-lg transition-colors">
								<span>+</span> FREE DAILY CASE
							</div>
						</div>
						<div class="flex-shrink-0">
							<img src="/keydrop-promo-code.png" alt="Key-Drop" class="w-20 h-20 object-cover rounded-lg opacity-80" />
						</div>
					</div>
				</a>
				
				<!-- Desktop Banner (iframe embed with full height) -->
					<a
						href="https://kd.link/?code=DIGIPULATION"
						target="_blank"
						rel="noopener noreferrer"
						aria-label="Key-Drop Promo - Get bonus with code DIGIPULATION"
						class="keydrop-banner-container hidden md:block overflow-hidden rounded-xl border border-white/10 hover:border-yellow-500/30 transition-colors duration-300"
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
		
		<h1 class="flex justify-center mb-4">
			<svg viewBox="0 0 450 70" class="w-full max-w-lg h-auto" xmlns="http://www.w3.org/2000/svg" aria-label="CS2 Giveaways Exclusives">
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
				
				<text x="225" y="32" text-anchor="middle" fill="url(#goldGradient)" font-family="Chakra Petch, sans-serif" font-weight="700" font-size="28" letter-spacing="2" filter="url(#glow)">
					CS2GIVEAWAYS
				</text>
				<text x="225" y="58" text-anchor="middle" fill="#ffffff" font-family="Chakra Petch, sans-serif" font-weight="600" font-size="18" letter-spacing="5" opacity="0.9">
					EXCLUSIVES
				</text>
			</svg>
		</h1>

		<div class="relative flex flex-col items-center">
			
			<div class="absolute inset-0 flex items-center justify-center pointer-events-none">
				<div class="w-[600px] h-[600px] rounded-full bg-yellow-500/5 blur-[100px]"></div>
				<div class="absolute w-[400px] h-[400px] rounded-full bg-purple-500/10 blur-[80px]"></div>
			</div>

			<button 
				onclick={rotateLeft}
				class="absolute left-4 md:left-8 top-1/2 -translate-y-1/2 z-20 w-12 h-12 rounded-full bg-black/50 border border-white/10 flex items-center justify-center text-gray-400 hover:text-yellow-500 hover:border-yellow-500/50 hover:bg-yellow-500/10 transition-all duration-300 active:scale-95 backdrop-blur-sm"
				aria-label="Previous offer"
			>
				<svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/>
				</svg>
			</button>
			
			<button 
				onclick={rotateRight}
				class="absolute right-4 md:right-8 top-1/2 -translate-y-1/2 z-20 w-12 h-12 rounded-full bg-black/50 border border-white/10 flex items-center justify-center text-gray-400 hover:text-yellow-500 hover:border-yellow-500/50 hover:bg-yellow-500/10 transition-all duration-300 active:scale-95 backdrop-blur-sm"
				aria-label="Next offer"
			>
				<svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
				</svg>
			</button>

			<!-- Desktop 3D Carousel -->
			<div 
				class="relative w-full max-w-5xl h-[550px] md:h-[620px] hidden md:block touch-none cursor-grab active:cursor-grabbing"
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
					class="relative w-full h-full select-none"
					style="transform-style: preserve-3d; transform: rotateY({wheelRotation}deg); transition: transform {isDragging ? '0ms' : '500ms'} ease-out;"
				>
					{#each exclusiveOffers as offer, index}
						{@const angle = index * anglePerItem}
						{@const translateZ = 220}
						<div 
							class="absolute top-1/2 left-1/2 w-[300px] -translate-x-1/2 -translate-y-1/2 pointer-events-auto"
							style="transform: rotateY({angle}deg) translateZ({translateZ}px);"
						>
							<a
								href={offer.claimUrl}
								target="_blank"
								rel="noopener noreferrer"
								class="group block relative rounded-2xl overflow-hidden shadow-2xl transition-all duration-500 hover:scale-105"
							>
								<!-- Outer glow on hover -->
								<div class="absolute -inset-1 bg-gradient-to-r from-yellow-500 via-yellow-400 to-yellow-500 rounded-2xl opacity-0 group-hover:opacity-75 blur-md transition-opacity duration-500"></div>
								
								<!-- Card container -->
								<div class="relative rounded-2xl overflow-hidden border-2 border-white/10 group-hover:border-yellow-500/50 transition-colors duration-300">
									
									<!-- Image -->
									<div class="relative aspect-[1024/1269] overflow-hidden">
										<img 
											src={offer.image} 
											alt="{offer.name} Exclusive Offer"
											class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110"
											draggable="false"
										/>
										
										<!-- Shine effect on hover -->
										<div class="absolute inset-0 bg-gradient-to-tr from-transparent via-white/20 to-transparent translate-x-[-100%] group-hover:translate-x-[100%] transition-transform duration-1000 ease-out"></div>
										
										<!-- Subtle vignette -->
										<div class="absolute inset-0 shadow-[inset_0_0_50px_rgba(0,0,0,0.3)] pointer-events-none"></div>
									</div>
									
									<!-- Bottom highlight bar -->
									<div class="absolute bottom-0 left-0 right-0 h-1 bg-gradient-to-r from-transparent via-yellow-500 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
								</div>
							</a>
						</div>
					{/each}
				</div>
			</div>

			<!-- Mobile Swipeable Cards -->
			<div 
				class="relative w-full md:hidden overflow-hidden touch-pan-y"
				onpointerdown={handleDragStart}
				onpointermove={handleDragMove}
				onpointerup={handleDragEnd}
				onpointercancel={handleDragEnd}
			>
				<div 
					class="flex gap-4 px-4 select-none py-4"
					style="transform: translateX(calc(50% - 150px - {getCurrentIndex() * 308}px)); transition: transform {isDragging ? '0ms' : '300ms'} ease-out;"
				>
					{#each exclusiveOffers as offer, index}
						{@const isActive = getCurrentIndex() === index}
						<a
							href={offer.claimUrl}
							target="_blank"
							rel="noopener noreferrer"
							class="flex-shrink-0 w-[300px] block relative group"
							style="opacity: {isActive ? 1 : 0.5}; transform: scale({isActive ? 1 : 0.9}); transition: opacity 300ms, transform 300ms;"
						>
							<!-- Outer glow -->
							<div class="absolute -inset-1 bg-gradient-to-r from-yellow-500 via-yellow-400 to-yellow-500 rounded-2xl opacity-0 {isActive ? 'group-hover:opacity-75' : ''} blur-md transition-opacity duration-500"></div>
							
							<!-- Card -->
							<div class="relative rounded-2xl overflow-hidden border-2 {isActive ? 'border-yellow-500/30' : 'border-white/10'} transition-colors duration-300">
								<div class="relative aspect-[1024/1269] overflow-hidden">
									<img 
										src={offer.image} 
										alt="{offer.name} Exclusive Offer"
										class="w-full h-full object-cover"
										draggable="false"
									/>
									
									<!-- Subtle vignette -->
									<div class="absolute inset-0 shadow-[inset_0_0_50px_rgba(0,0,0,0.3)] pointer-events-none"></div>
								</div>
								
								<!-- Bottom highlight bar for active -->
								{#if isActive}
									<div class="absolute bottom-0 left-0 right-0 h-1 bg-gradient-to-r from-transparent via-yellow-500 to-transparent"></div>
								{/if}
							</div>
						</a>
					{/each}
				</div>
			</div>

			<div class="flex gap-3 mt-6">
				{#each exclusiveOffers as _, index}
					<button
						onclick={() => goToSlide(index)}
						class="h-3 rounded-full transition-all duration-300 {getCurrentIndex() === index ? 'bg-yellow-500 w-8' : 'bg-white/20 hover:bg-white/40 w-3'}"
						aria-label="Go to offer {index + 1}"
					></button>
				{/each}
			</div>
		</div>
	</section>

	<div class="flex items-center gap-4">
		<div class="flex-1 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
		<span class="text-gray-600 text-sm uppercase tracking-wider">How It Works</span>
		<div class="flex-1 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
	</div>

	<section class="relative">
		<div class="text-center mb-12">
			<h2 class="font-display font-bold text-3xl md:text-4xl text-white mb-4">
				Win <span class="text-yellow-500">CS2 Skins</span> in 3 Easy Steps
			</h2>
			<p class="text-gray-400 max-w-2xl mx-auto">
				Join thousands of players who have already won free CS2 skins through our curated giveaways and exclusive partner offers.
			</p>
		</div>

		<div class="grid md:grid-cols-3 gap-6 lg:gap-8">
			<!-- Step 1 -->
			<div class="group relative">
				<div class="absolute -inset-0.5 bg-gradient-to-r from-yellow-500 to-yellow-600 rounded-2xl opacity-0 group-hover:opacity-20 blur transition-opacity duration-300"></div>
				<div class="relative bg-[#1a1d24] border border-white/10 rounded-2xl p-6 h-full hover:border-yellow-500/30 transition-colors duration-300">
					<div class="w-14 h-14 rounded-xl bg-yellow-500/10 border border-yellow-500/20 flex items-center justify-center mb-5">
						<span class="text-2xl font-bold text-yellow-500">1</span>
					</div>
					<h3 class="text-xl font-semibold text-white mb-3">Browse Offers</h3>
					<p class="text-gray-400 text-sm leading-relaxed">
						Explore our exclusive partner offers above. Each offer gives you free bonuses, cases, or coins when you sign up using our codes.
					</p>
					<div class="mt-4 pt-4 border-t border-white/5">
						<div class="flex items-center gap-2 text-xs text-gray-500">
							<svg class="w-4 h-4 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>
							</svg>
							<span>Verified & trusted partners</span>
						</div>
					</div>
				</div>
			</div>

			<!-- Step 2 -->
			<div class="group relative">
				<div class="absolute -inset-0.5 bg-gradient-to-r from-yellow-500 to-yellow-600 rounded-2xl opacity-0 group-hover:opacity-20 blur transition-opacity duration-300"></div>
				<div class="relative bg-[#1a1d24] border border-white/10 rounded-2xl p-6 h-full hover:border-yellow-500/30 transition-colors duration-300">
					<div class="w-14 h-14 rounded-xl bg-yellow-500/10 border border-yellow-500/20 flex items-center justify-center mb-5">
						<span class="text-2xl font-bold text-yellow-500">2</span>
					</div>
					<h3 class="text-xl font-semibold text-white mb-3">Claim Your Bonus</h3>
					<p class="text-gray-400 text-sm leading-relaxed">
						Click on any offer to visit the site. Sign up or log in and use our promo code to instantly receive your free bonus rewards.
					</p>
					<div class="mt-4 pt-4 border-t border-white/5">
						<div class="flex items-center gap-2 text-xs text-gray-500">
							<svg class="w-4 h-4 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
							</svg>
							<span>Instant bonus activation</span>
						</div>
					</div>
				</div>
			</div>

			<!-- Step 3 -->
			<div class="group relative">
				<div class="absolute -inset-0.5 bg-gradient-to-r from-yellow-500 to-yellow-600 rounded-2xl opacity-0 group-hover:opacity-20 blur transition-opacity duration-300"></div>
				<div class="relative bg-[#1a1d24] border border-white/10 rounded-2xl p-6 h-full hover:border-yellow-500/30 transition-colors duration-300">
					<div class="w-14 h-14 rounded-xl bg-yellow-500/10 border border-yellow-500/20 flex items-center justify-center mb-5">
						<span class="text-2xl font-bold text-yellow-500">3</span>
					</div>
					<h3 class="text-xl font-semibold text-white mb-3">Win & Withdraw</h3>
					<p class="text-gray-400 text-sm leading-relaxed">
						Use your bonuses to play games, open cases, or enter giveaways. Win CS2 skins and withdraw them directly to your Steam account.
					</p>
					<div class="mt-4 pt-4 border-t border-white/5">
						<div class="flex items-center gap-2 text-xs text-gray-500">
							<svg class="w-4 h-4 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/>
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
				class="absolute inset-0 bg-black/80 backdrop-blur-sm cursor-default"
				onclick={closePrizeModal}
				aria-label="Close modal"
			></button>
			
			<!-- Modal -->
			<div class="relative bg-gradient-to-br from-[#1a1d24] to-[#12141a] border border-white/20 rounded-2xl p-8 max-w-md w-full text-center transform animate-bounce-in">
				<!-- Confetti effect -->
				<div class="absolute inset-0 overflow-hidden rounded-2xl pointer-events-none">
					{#each Array(20) as _, i}
						<div
							class="absolute w-2 h-2 rounded-full animate-confetti"
							style="
								left: {Math.random() * 100}%;
								background: {['#eab308', '#22c55e', '#3b82f6', '#ef4444', '#a855f7'][i % 5]};
								animation-delay: {Math.random() * 0.5}s;
							"
						></div>
					{/each}
				</div>
				
				<!-- Content -->
				<div class="relative z-10">
					<div class="w-20 h-20 mx-auto mb-6 rounded-full flex items-center justify-center" style="background: {wonPrize.color}">
						<svg class="w-10 h-10 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7"/>
						</svg>
					</div>
					
					<h3 class="text-2xl font-bold text-white mb-2">🎉 Congratulations!</h3>
					<p class="text-gray-400 mb-6">You won:</p>
					
					<div class="bg-white/5 border border-white/10 rounded-xl p-4 mb-6">
						<p class="text-yellow-500 font-bold text-xl">{wonPrize.reward}</p>
						<p class="text-gray-300">at {wonPrize.name}</p>
					</div>
					
					<div class="flex gap-3">
						<button
							onclick={closePrizeModal}
							class="flex-1 px-6 py-3 bg-gray-700 hover:bg-gray-600 text-white rounded-full font-medium transition-colors"
						>
							Maybe Later
						</button>
						<button
							onclick={claimPrize}
							class="flex-1 px-6 py-3 bg-gradient-to-r from-yellow-500 to-yellow-600 hover:from-yellow-400 hover:to-yellow-500 text-black rounded-full font-bold transition-all"
						>
							Claim Now →
						</button>
					</div>
					
					<p class="text-gray-500 text-xs mt-4">
						Click "Claim Now" to visit {wonPrize.name} and get your free case!
					</p>
				</div>
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