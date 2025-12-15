<script>
	import { onMount } from 'svelte';
	
	let wheelRotation = $state(0);
	let isDragging = $state(false);
	let startX = $state(0);
	let startRotation = $state(0);
	let wheelElement = $state(null);

	// Load Twitter widgets script
	onMount(() => {
		// Check if the script is already loaded
		if (!window.twttr) {
			const script = document.createElement('script');
			script.async = true;
			script.src = 'https://platform.twitter.com/widgets.js';
			script.charset = 'utf-8';
			document.body.appendChild(script);
			
			// Initialize widgets when script loads
			script.onload = () => {
				if (window.twttr && window.twttr.widgets) {
					window.twttr.widgets.load();
				}
			};
		} else if (window.twttr && window.twttr.widgets) {
			// If already loaded, just refresh the widgets
			window.twttr.widgets.load();
		}
	});

	const exclusiveOffers = [
		{
			id: 1,
			name: 'CSGOEmpire',
			image: '/csgoempire-affiliate.png',
			claimUrl: 'https://csgoempire.com/r/catweazle'
		},
		{
			id: 2,
			name: 'Key-Drop',
			image: '/keydrop-promo-code.png',
			claimUrl: 'https://kd.link/?code=CATWZ'
		},
		{
			id: 3,
			name: 'Clash.gg',
			image: '/clashgg-affiliate-offer.jpg',
			claimUrl: 'https://clash.gg/r/CATW'
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

<div class="container mx-auto max-w-6xl px-4 py-12 space-y-16">

	<section class="relative">
		
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
					CS2GIVEAWAYS
				</text>
				<text x="225" y="58" text-anchor="middle" fill="#ffffff" font-family="Rajdhani, sans-serif" font-weight="600" font-size="18" letter-spacing="5" opacity="0.9">
					EXCLUSIVES
				</text>
			</svg>
		</div>

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
											alt={offer.name}
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
										alt={offer.name}
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
		<span class="text-gray-600 text-sm uppercase tracking-wider">Live Giveaways</span>
		<div class="flex-1 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"></div>
	</div>

	<section class="relative">
		<div class="flex flex-col lg:flex-row gap-8">
			
			<div class="lg:w-1/3 space-y-6">
				<div class="space-y-3">
					<h2 class="font-display font-bold text-3xl text-white">
						Live <span class="text-yellow-500">Giveaways</span>
					</h2>
					<p class="text-gray-400 leading-relaxed">
						Stay updated with the latest CS2 skin giveaways from the community. We aggregate the best opportunities so you never miss a chance to win.
					</p>
				</div>
				
				<div class="space-y-3">
					<div class="flex items-center gap-3 text-sm">
						<span class="w-8 h-8 rounded-full bg-green-500/20 text-green-400 flex items-center justify-center">
							<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
						</span>
						<span class="text-gray-300">Updated in real-time</span>
					</div>
					<div class="flex items-center gap-3 text-sm">
						<span class="w-8 h-8 rounded-full bg-green-500/20 text-green-400 flex items-center justify-center">
							<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
						</span>
						<span class="text-gray-300">Verified giveaways only</span>
					</div>
					<div class="flex items-center gap-3 text-sm">
						<span class="w-8 h-8 rounded-full bg-green-500/20 text-green-400 flex items-center justify-center">
							<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
						</span>
						<span class="text-gray-300">From trusted sources</span>
					</div>
				</div>

				<a 
					href="/giveaways" 
					class="inline-flex items-center gap-2 text-yellow-500 hover:text-yellow-400 font-medium transition-colors"
				>
					View all giveaways
					<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 8l4 4m0 0l-4 4m4-4H3"></path></svg>
				</a>
			</div>
			
			<div class="lg:w-2/3">
				<div class="bg-[#1a1d24] border border-white/10 rounded-xl overflow-hidden min-h-[500px]">
					<div class="p-4 border-b border-white/10 flex items-center gap-3">
						<svg class="w-6 h-6 text-white" viewBox="0 0 24 24" fill="currentColor">
							<path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
						</svg>
						<span class="text-white font-medium">CS2 Giveaways Feed</span>
					</div>
					
					<div class="p-4 min-h-[450px]" id="twitter-feed-container">
						<!-- Twitter Timeline Widget -->
						<a
							class="twitter-timeline"
							data-lang="en"
							data-theme="dark"
							data-height="450"
							data-chrome="noheader nofooter noborders transparent"
							href="https://twitter.com/cs2giveawayspro?ref_src=twsrc%5Etfw"
						>
							<!-- Fallback content while loading -->
							<div class="text-center space-y-4 text-gray-500 py-8">
								<svg class="w-12 h-12 mx-auto opacity-50 animate-pulse" viewBox="0 0 24 24" fill="currentColor">
									<path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
								</svg>
								<p class="text-sm">Loading tweets from @cs2giveawayspro...</p>
							</div>
						</a>
					</div>
				</div>
			</div>
		</div>
	</section>

</div>