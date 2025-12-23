<script>
	import { page } from '$app/stores';
</script>

<svelte:head>
	<title>CS2 Giveaways - Frequently Asked Questions</title>
	<link rel="icon" href="/favicon.png" />
</svelte:head>

<style>
	.faq-card {
		background: linear-gradient(135deg, rgba(30, 30, 40, 0.6) 0%, rgba(20, 20, 28, 0.8) 100%);
		border: 1px solid rgba(234, 179, 8, 0.1);
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		position: relative;
		overflow: hidden;
	}
	
	.faq-card::before {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(234, 179, 8, 0.05), transparent);
		transition: left 0.5s;
	}
	
	.faq-card:hover {
		border-color: rgba(234, 179, 8, 0.3);
		transform: translateY(-2px);
		box-shadow: 0 8px 24px rgba(234, 179, 8, 0.15);
	}
	
	.faq-card:hover::before {
		left: 100%;
	}
	
	.faq-number {
		background: linear-gradient(135deg, rgba(234, 179, 8, 0.2) 0%, rgba(234, 179, 8, 0.05) 100%);
		border: 1px solid rgba(234, 179, 8, 0.3);
	}
	
	.gradient-text {
		background: linear-gradient(135deg, #eab308 0%, #fbbf24 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		background-clip: text;
	}
	
	.header-glow {
		text-shadow: 0 0 40px rgba(234, 179, 8, 0.3);
	}
	
	@keyframes fadeInUp {
		from {
			opacity: 0;
			transform: translateY(20px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}
	
	.fade-in-up {
		animation: fadeInUp 0.6s ease-out forwards;
	}
</style>

<div class="app-container min-h-screen bg-[#0f1115] text-gray-300 font-sans selection:bg-yellow-500/30 selection:text-yellow-200 flex flex-col">
	<main class="flex-grow relative z-0">
		<!-- Background decoration -->
		<div class="absolute inset-0 overflow-hidden pointer-events-none">
			<div class="absolute top-0 left-1/4 w-96 h-96 bg-yellow-500/5 rounded-full blur-3xl"></div>
			<div class="absolute bottom-0 right-1/4 w-96 h-96 bg-yellow-500/5 rounded-full blur-3xl"></div>
		</div>
		
		<div class="container mx-auto max-w-5xl px-4 py-12 relative z-10">
			<!-- Header Section -->
			<div class="text-center mb-16 fade-in-up">
				<h1 class="text-5xl md:text-6xl font-display font-bold text-white mb-4 header-glow">
					Frequently Asked <span class="gradient-text">Questions</span>
				</h1>
				<p class="text-gray-400 text-lg max-w-2xl mx-auto">
					Everything you need to know about CS2 giveaways, promo codes, and free cases
				</p>
			</div>
			
			<!-- FAQ Grid -->
			<div class="space-y-6">
				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.1s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							01
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								How do I use the exclusive offers featured in the carousel?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								To claim an offer, select the image or link in the carousel. A single click will redirect you to the partner site where you can claim the exclusive promotion. The carousel includes offers from sites like CSGOEmpire, Key-Drop, and Clash.gg.
							</p>
						</div>
					</div>
				</div>

				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.2s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							02
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								How often are the promo codes and giveaways updated?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								Promo codes and giveaways are updated daily. The site aggregates the best opportunities to ensure you do not miss a chance to win.
							</p>
						</div>
					</div>
				</div>

				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.3s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							03
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								How does CS2Giveaways verify that the listed giveaways are legitimate?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								Verification is done through direct contact and collaboration with reputable companies and trusted sources. Only verified giveaways are featured on the site.
							</p>
						</div>
					</div>
				</div>

				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.4s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							04
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								Where can I find the best CS2 promo codes on your site?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								The primary destination for all promo codes is the Promo Codes section. This section is accessible via the main navigation menu.
							</p>
						</div>
					</div>
				</div>

				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.5s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							05
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								What are "Free Cases," and how do I claim them?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								Free Cases are promotional offers provided by various companies and websites within the Counter-Strike scene. You can find these offers listed in the Free Cases section of the navigation.
							</p>
						</div>
					</div>
				</div>

				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.6s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							06
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								What kind of CS2 skins can I win?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								The value of skins won in giveaways can vary significantly. Winnings range from affordable skins to items worth hundreds of dollars, depending on the specific giveaway and luck.
							</p>
						</div>
					</div>
				</div>

				<div class="faq-card rounded-xl p-6 md:p-8 fade-in-up" style="animation-delay: 0.7s">
					<div class="flex items-start gap-4">
						<div class="faq-number flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center text-yellow-500 font-bold">
							07
						</div>
						<div>
							<h2 class="text-xl md:text-2xl font-display font-bold text-white mb-3">
								Is this site affiliated with Valve Corporation?
							</h2>
							<p class="text-gray-400 text-base md:text-lg leading-relaxed">
								The site is not affiliated with Valve Corporation.
							</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	</main>
</div>