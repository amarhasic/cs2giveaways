<script>
	import { onMount } from 'svelte';
	
	let visibleSteps = $state([]);
	let totalValue = $state(0);
	let animatedValue = $state(0);
	
	const offers = [
		{
			id: 1,
			site: 'CSGOEmpire',
			reward: '1 Free Case',
			value: 0.50,
			description: 'The OG CS2 gambling site. Sign up and claim your free case instantly!',
			steps: [
				'Click the button below to visit CSGOEmpire',
				'Sign in with your Steam account',
				'Your free case will be automatically credited!'
			],
			url: 'https://csgoempire.com/r/digipulation',
			color: 'from-red-600 to-red-800',
			bgColor: 'bg-red-500/10',
			borderColor: 'border-red-500/30',
			icon: '👑',
			tag: 'INSTANT'
		},
		{
			id: 2,
			site: 'Key-Drop',
			reward: '1 Free Case',
			value: 0.50,
			description: 'One of the biggest case opening sites. Get your free case with our code!',
			steps: [
				'Click the button to go to Key-Drop',
				'Create an account or log in',
				'Claim your free case!'
			],
			url: 'https://kd.link/?code=DIGIPULATION',
			color: 'from-blue-600 to-blue-800',
			bgColor: 'bg-blue-500/10',
			borderColor: 'border-blue-500/30',
			icon: '🔑',
			tag: 'POPULAR'
		},
		{
			id: 3,
			site: 'Howl.gg',
			reward: '1 Free Case',
			value: 0.50,
			description: 'Premium case battles and upgrades. Start with a free case on us!',
			steps: [
				'Visit Howl.gg through our link',
				'Connect your Steam account',
				'Your free case is waiting for you!'
			],
			url: 'https://howl.gg/r/dpl',
			color: 'from-purple-600 to-purple-800',
			bgColor: 'bg-purple-500/10',
			borderColor: 'border-purple-500/30',
			icon: '🐺',
			tag: 'NEW'
		},
		{
			id: 4,
			site: 'Clash.gg',
			reward: '3 Free Cases',
			value: 1.50,
			description: 'The best value! Get 3 FREE cases just for signing up. Triple the chances!',
			steps: [
				'Click to visit Clash.gg',
				'Sign up with Steam',
				'Go to the affiliate page',
				'Claim your 3 FREE cases!'
			],
			url: 'https://clash.gg/r/DIGIPULATION',
			color: 'from-green-600 to-green-800',
			bgColor: 'bg-green-500/10',
			borderColor: 'border-green-500/30',
			icon: '⚔️',
			tag: 'BEST VALUE'
		},
		{
			id: 5,
			site: 'DatDrop',
			reward: 'Daily Free Case',
			value: 0.50,
			description: 'Get a FREE case every single day! Come back daily for more chances to win!',
			steps: [
				'Visit DatDrop through our link',
				'Create your account',
				'Claim your daily free case',
				'Come back tomorrow for another!'
			],
			url: 'https://datdrop.com/p/digipulation',
			color: 'from-orange-600 to-orange-800',
			bgColor: 'bg-orange-500/10',
			borderColor: 'border-orange-500/30',
			icon: '📦',
			tag: 'DAILY'
		}
	];
	
	onMount(() => {
		// Calculate total value
		totalValue = offers.reduce((sum, offer) => sum + offer.value, 0);
		
		// Animate the value counter
		const duration = 2000;
		const steps = 60;
		const increment = totalValue / steps;
		let current = 0;
		
		const interval = setInterval(() => {
			current += increment;
			if (current >= totalValue) {
				animatedValue = totalValue;
				clearInterval(interval);
			} else {
				animatedValue = current;
			}
		}, duration / steps);
		
		// Animate steps appearing
		offers.forEach((_, index) => {
			setTimeout(() => {
				visibleSteps = [...visibleSteps, index];
			}, 200 * (index + 1));
		});
		
		return () => clearInterval(interval);
	});
	
	function handleClaim(url) {
		window.open(url, '_blank');
	}
	
	function scrollToTop() {
		window.scrollTo({ top: 0, behavior: 'smooth' });
	}
</script>

<svelte:head>
	<title>Free Skins Path - Get Free CS2 Cases & Skins | CS2 Giveaways</title>
	<meta name="description" content="Follow our free skins path to claim 7+ free CS2 cases worth over $3.50! Get free cases from CSGOEmpire, Key-Drop, Howl.gg, Clash.gg, and DatDrop." />
</svelte:head>

<div class="min-h-screen bg-gradient-to-b from-[#0f1115] via-[#12141a] to-[#0f1115]">
	<!-- Hero Section -->
	<section class="relative overflow-hidden py-16 md:py-24">
		<!-- Animated background -->
		<div class="absolute inset-0 overflow-hidden">
			<div class="absolute top-1/4 left-1/4 w-96 h-96 bg-yellow-500/10 rounded-full blur-[120px] animate-pulse"></div>
			<div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-purple-500/10 rounded-full blur-[120px] animate-pulse" style="animation-delay: 1s;"></div>
			<div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-green-500/5 rounded-full blur-[150px]"></div>
		</div>
		
		<!-- Floating particles -->
		<div class="absolute inset-0 overflow-hidden pointer-events-none">
			{#each Array(20) as _, i}
				<div
					class="absolute w-1 h-1 bg-yellow-400/40 rounded-full animate-float"
					style="left: {Math.random() * 100}%; top: {Math.random() * 100}%; animation-delay: {Math.random() * 5}s; animation-duration: {3 + Math.random() * 4}s;"
				></div>
			{/each}
		</div>
		
		<div class="container mx-auto max-w-6xl px-4 relative z-10">
			<div class="text-center">
				<!-- Badge -->
				<div class="inline-flex items-center gap-2 px-4 py-2 bg-yellow-500/10 border border-yellow-500/30 rounded-full mb-6">
					<span class="relative flex h-2 w-2">
						<span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
						<span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
					</span>
					<span class="text-yellow-400 text-sm font-medium">100% FREE - No Deposit Required</span>
				</div>
				
				<!-- Main heading -->
				<h1 class="text-4xl md:text-6xl lg:text-7xl font-bold mb-6">
					<span class="text-white">Your Path to</span>
					<br />
					<span class="bg-gradient-to-r from-yellow-400 via-yellow-500 to-orange-500 bg-clip-text text-transparent">
						FREE CS2 SKINS
					</span>
				</h1>
				
				<p class="text-gray-400 text-lg md:text-xl max-w-2xl mx-auto mb-8">
					Follow this guide to claim <span class="text-white font-semibold">7+ free cases</span> from the best CS2 sites. 
					No tricks, no deposits - just free skins waiting for you!
				</p>
				
				<!-- Value counter -->
					<div class="inline-flex flex-col items-center gap-2 p-6 bg-gradient-to-br from-yellow-500/20 to-orange-500/10 border border-yellow-500/30 rounded-2xl">
						<span class="text-gray-400 text-sm uppercase tracking-wider">Potential Winnings</span>
						<div class="flex items-baseline gap-1">
							<span class="text-3xl md:text-4xl font-bold text-yellow-400">$0.50</span>
							<span class="text-2xl md:text-3xl font-bold text-gray-500 mx-2">to</span>
							<span class="text-3xl md:text-4xl font-bold text-green-400">$100s</span>
						</div>
						<span class="text-gray-400 text-sm font-medium">Open cases, win skins, cash out</span>
					</div>
				
				<!-- Scroll indicator -->
				<div class="mt-12 animate-bounce">
					<svg class="w-8 h-8 mx-auto text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3"/>
					</svg>
					<span class="text-gray-500 text-sm">Scroll to start your journey</span>
				</div>
			</div>
		</div>
	</section>
	
	<!-- Path Section -->
	<section class="py-16 relative">
		<div class="container mx-auto max-w-4xl px-4">
			<!-- Section header -->
				<div class="text-center mb-16">
					<h2 class="text-3xl md:text-4xl font-bold text-white mb-4">
						Your Free Skins Journey
					</h2>
					<p class="text-gray-400">Complete each step to maximize your free rewards</p>
				</div>
			
			<!-- Path timeline -->
			<div class="relative">
				<!-- Vertical line -->
				<div class="absolute left-8 md:left-1/2 top-0 bottom-0 w-1 bg-gradient-to-b from-yellow-500 via-purple-500 to-green-500 rounded-full md:-translate-x-1/2"></div>
				
				<!-- Offer cards -->
				{#each offers as offer, index}
					<div 
						class="relative mb-12 last:mb-0 transition-all duration-700 {visibleSteps.includes(index) ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
						style="transition-delay: {index * 100}ms;"
					>
						<!-- Step number -->
						<div class="absolute left-8 md:left-1/2 -translate-x-1/2 w-16 h-16 rounded-full bg-gradient-to-br {offer.color} flex items-center justify-center text-3xl shadow-lg shadow-black/50 z-10 border-4 border-[#12141a]">
							{offer.icon}
						</div>
						
						<!-- Card -->
						<div class="ml-20 md:ml-0 md:w-[calc(50%-3rem)] {index % 2 === 0 ? 'md:mr-auto md:pr-8' : 'md:ml-auto md:pl-8'}">
							<div class="group relative">
								<!-- Glow effect -->
								<div class="absolute -inset-1 bg-gradient-to-r {offer.color} rounded-2xl opacity-0 group-hover:opacity-50 blur-lg transition-opacity duration-500"></div>
								
								<!-- Card content -->
								<div class="relative {offer.bgColor} border {offer.borderColor} rounded-2xl p-6 hover:border-opacity-60 transition-all duration-300">
									<!-- Tag -->
									<div class="absolute -top-3 right-4">
										<span class="px-3 py-1 bg-gradient-to-r {offer.color} text-white text-xs font-bold rounded-full shadow-lg">
											{offer.tag}
										</span>
									</div>
									
									<!-- Header -->
									<div class="flex items-start justify-between mb-4">
										<div>
											<h3 class="text-xl font-bold text-white mb-1">{offer.site}</h3>
											<div class="flex items-center gap-2">
												<span class="text-2xl font-bold text-yellow-400">{offer.reward}</span>
												<span class="text-gray-500 text-sm">(~${offer.value.toFixed(2)})</span>
											</div>
										</div>
									</div>
									
									<!-- Description -->
									<p class="text-gray-400 text-sm mb-4">{offer.description}</p>
									
									<!-- Steps -->
									<div class="space-y-2 mb-6">
										{#each offer.steps as step, stepIndex}
											<div class="flex items-start gap-3">
												<div class="flex-shrink-0 w-6 h-6 rounded-full bg-white/10 flex items-center justify-center text-xs text-gray-400">
													{stepIndex + 1}
												</div>
												<span class="text-gray-300 text-sm">{step}</span>
											</div>
										{/each}
									</div>
									
									<!-- CTA Button -->
										<button
											onclick={() => handleClaim(offer.url)}
											class="w-full py-4 px-6 bg-gradient-to-r {offer.color} hover:opacity-90 text-white font-bold rounded-xl transition-all duration-300 transform hover:scale-[1.02] active:scale-[0.98] shadow-lg flex items-center justify-center gap-2 cursor-pointer"
										>
										<span>Claim {offer.reward}</span>
										<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
											<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
										</svg>
									</button>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>
	
	<!-- Summary Section -->
	<section class="py-16 relative">
		<div class="container mx-auto max-w-4xl px-4">
			<div class="relative">
				<!-- Glow -->
				<div class="absolute -inset-4 bg-gradient-to-r from-yellow-500/20 via-green-500/20 to-purple-500/20 rounded-3xl blur-2xl"></div>
				
				<!-- Card -->
				<div class="relative bg-gradient-to-br from-[#1a1d24] to-[#12141a] border border-white/10 rounded-3xl p-8 md:p-12">
					<div class="text-center mb-8">
							<h2 class="text-3xl md:text-4xl font-bold text-white mb-4">
								Complete the Path & Win Big!
							</h2>
							<p class="text-gray-400 max-w-2xl mx-auto">
								By following all the steps above, you'll have claimed free cases from 5 different sites.
								That's multiple chances to win valuable CS2 skins - completely FREE!
							</p>
						</div>
					
					<!-- Stats grid -->
						<div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
							<div class="bg-white/5 rounded-xl p-4 text-center">
								<div class="text-3xl font-bold text-yellow-400 mb-1">7+</div>
								<div class="text-gray-400 text-sm">Free Cases</div>
							</div>
							<div class="bg-white/5 rounded-xl p-4 text-center">
								<div class="text-3xl font-bold text-green-400 mb-1">$100s</div>
								<div class="text-gray-400 text-sm">Potential Wins</div>
							</div>
							<div class="bg-white/5 rounded-xl p-4 text-center">
								<div class="text-3xl font-bold text-purple-400 mb-1">5</div>
								<div class="text-gray-400 text-sm">Top Sites</div>
							</div>
							<div class="bg-white/5 rounded-xl p-4 text-center">
								<div class="text-3xl font-bold text-orange-400 mb-1">24/7</div>
								<div class="text-gray-400 text-sm">New Rewards</div>
							</div>
						</div>
					
					<!-- Pro tips -->
						<div class="bg-yellow-500/10 border border-yellow-500/30 rounded-xl p-6">
							<h3 class="text-lg font-bold text-yellow-400 mb-4">
								Pro Tips to Maximize Your Winnings
							</h3>
						<ul class="space-y-3 text-gray-300 text-sm">
							<li class="flex items-start gap-3">
								<span class="text-green-400">✓</span>
								<span>Complete all 5 sites to get the maximum number of free cases</span>
							</li>
							<li class="flex items-start gap-3">
								<span class="text-green-400">✓</span>
								<span>Return to DatDrop daily for your free daily case</span>
							</li>
							<li class="flex items-start gap-3">
								<span class="text-green-400">✓</span>
								<span>Check each site for additional daily rewards and bonuses</span>
							</li>
							<li class="flex items-start gap-3">
								<span class="text-green-400">✓</span>
								<span>Join case battles for a chance to multiply your winnings</span>
							</li>
						</ul>
					</div>
				</div>
			</div>
		</div>
	</section>
	
	<!-- FAQ Section -->
	<section class="py-16">
		<div class="container mx-auto max-w-4xl px-4">
			<h2 class="text-3xl font-bold text-white text-center mb-12">
				Frequently Asked Questions
			</h2>
			
			<div class="space-y-4">
				<div class="bg-white/5 border border-white/10 rounded-xl p-6">
					<h3 class="text-lg font-semibold text-white mb-2">Is this really free?</h3>
					<p class="text-gray-400">Yes! All the cases mentioned are completely free. You don't need to deposit any money. Just sign up through our links and claim your rewards.</p>
				</div>
				
				<div class="bg-white/5 border border-white/10 rounded-xl p-6">
					<h3 class="text-lg font-semibold text-white mb-2">How do I withdraw my skins?</h3>
					<p class="text-gray-400">Each site has its own withdrawal system. Generally, you can withdraw skins directly to your Steam account once you've won them from the cases.</p>
				</div>
				
				<div class="bg-white/5 border border-white/10 rounded-xl p-6">
					<h3 class="text-lg font-semibold text-white mb-2">Can I claim from all sites?</h3>
					<p class="text-gray-400">Absolutely! Each site is independent, so you can claim free cases from all 5 sites. That's why we recommend completing the entire path!</p>
				</div>
				
				<div class="bg-white/5 border border-white/10 rounded-xl p-6">
					<h3 class="text-lg font-semibold text-white mb-2">Why do you share these codes?</h3>
					<p class="text-gray-400">We're affiliates of these sites. When you sign up through our links, we earn a small commission - but you get free cases! It's a win-win.</p>
				</div>
			</div>
		</div>
	</section>
	
	<!-- CTA Section -->
	<section class="py-16">
		<div class="container mx-auto max-w-4xl px-4 text-center">
			<div class="inline-flex items-center gap-2 px-4 py-2 bg-green-500/10 border border-green-500/30 rounded-full mb-6">
				<span class="text-green-400 text-sm font-medium">Start claiming now - it only takes 5 minutes!</span>
			</div>
			
			<h2 class="text-3xl md:text-4xl font-bold text-white mb-6">
				Ready to Get Your Free Skins?
			</h2>
			
			<p class="text-gray-400 mb-8 max-w-xl mx-auto">
				Scroll back up and start with the first site. Complete all 5 to maximize your free rewards!
			</p>
			
			<button
					onclick={scrollToTop}
					class="px-8 py-4 bg-gradient-to-r from-yellow-500 to-orange-500 hover:from-yellow-400 hover:to-orange-400 text-black font-bold rounded-full transition-all duration-300 transform hover:scale-105 shadow-lg shadow-yellow-500/25 cursor-pointer"
				>
					Start Your Free Skins Journey →
				</button>
		</div>
	</section>
</div>

<style>
	@keyframes float {
		0%, 100% {
			transform: translateY(0) translateX(0);
			opacity: 0.4;
		}
		50% {
			transform: translateY(-20px) translateX(10px);
			opacity: 0.8;
		}
	}
	
	.animate-float {
		animation: float 4s ease-in-out infinite;
	}
</style>