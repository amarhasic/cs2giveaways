<script>
	import { page } from '$app/stores';
	
	let emailRevealed = false;
	
	function revealEmail() {
		emailRevealed = true;
	}
</script>

<svelte:head>
	<title>CS2 Giveaways - Contact Us</title>
	<link rel="icon" href="/favicon.png" />
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
</svelte:head>

<style>
	.contact-card {
		background: linear-gradient(135deg, rgba(30, 30, 40, 0.6) 0%, rgba(20, 20, 28, 0.8) 100%);
		border: 1px solid rgba(234, 179, 8, 0.1);
		transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
		position: relative;
		overflow: hidden;
	}
	
	.contact-card::before {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(234, 179, 8, 0.05), transparent);
		transition: left 0.5s;
	}
	
	.contact-card:hover {
		border-color: rgba(234, 179, 8, 0.3);
		transform: translateY(-2px);
		box-shadow: 0 8px 24px rgba(234, 179, 8, 0.15);
	}
	
	.contact-card:hover::before {
		left: 100%;
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
	
	.email-link {
		color: #eab308;
		text-decoration: none;
		position: relative;
		transition: all 0.3s ease;
		padding: 0.5rem 1.5rem;
		border-radius: 0.5rem;
		background: rgba(234, 179, 8, 0.1);
		border: 1px solid rgba(234, 179, 8, 0.3);
		display: inline-block;
		font-weight: 600;
	}
	
	.email-link:hover {
		background: rgba(234, 179, 8, 0.2);
		border-color: rgba(234, 179, 8, 0.5);
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(234, 179, 8, 0.3);
	}
	
	.reveal-button {
		background: linear-gradient(135deg, rgba(234, 179, 8, 0.2) 0%, rgba(234, 179, 8, 0.1) 100%);
		border: 1px solid rgba(234, 179, 8, 0.3);
		color: #eab308;
		padding: 0.75rem 2rem;
		border-radius: 0.5rem;
		font-weight: 600;
		cursor: pointer;
		transition: all 0.3s ease;
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
	}
	
	.reveal-button:hover {
		background: linear-gradient(135deg, rgba(234, 179, 8, 0.3) 0%, rgba(234, 179, 8, 0.2) 100%);
		border-color: rgba(234, 179, 8, 0.5);
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(234, 179, 8, 0.3);
	}
	
	.icon-wrapper {
		background: linear-gradient(135deg, rgba(234, 179, 8, 0.2) 0%, rgba(234, 179, 8, 0.05) 100%);
		border: 1px solid rgba(234, 179, 8, 0.3);
		transition: all 0.3s ease;
	}
	
	.contact-card:hover .icon-wrapper {
		transform: scale(1.1);
		box-shadow: 0 0 20px rgba(234, 179, 8, 0.4);
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
	
	@keyframes float {
		0%, 100% { transform: translateY(0px); }
		50% { transform: translateY(-10px); }
	}
	
	.float-animation {
		animation: float 3s ease-in-out infinite;
	}
	
	@keyframes fadeIn {
		from { opacity: 0; }
		to { opacity: 1; }
	}
	
	.fade-in {
		animation: fadeIn 0.4s ease-out forwards;
	}
</style>

<div class="app-container bg-[#0f1115] text-gray-300 font-sans selection:bg-yellow-500/30 selection:text-yellow-200">
	<main class="relative z-0">
		<!-- Background decoration -->
		<div class="absolute inset-0 overflow-hidden pointer-events-none">
			<div class="absolute top-1/4 left-1/4 w-96 h-96 bg-yellow-500/5 rounded-full blur-3xl"></div>
			<div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-yellow-500/5 rounded-full blur-3xl"></div>
		</div>
		
		<div class="container mx-auto max-w-4xl px-4 py-12 pb-8 relative z-10">
			<!-- Header Section -->
			<div class="text-center mb-12 fade-in-up">
				<h1 class="text-5xl md:text-6xl font-display font-bold text-white mb-4 header-glow">
					Get in <span class="gradient-text">Touch</span>
				</h1>
				<p class="text-gray-400 text-lg max-w-2xl mx-auto">
					Have questions or need assistance? We're here to help
				</p>
			</div>
			
			<!-- Contact Card -->
			<div class="contact-card rounded-2xl p-8 md:p-12 mb-8 fade-in-up" style="animation-delay: 0.2s">
				<div class="text-center">
					<!-- Email Icon -->
					<div class="flex justify-center mb-8">
						<div class="icon-wrapper w-20 h-20 rounded-2xl flex items-center justify-center float-animation">
							<svg class="w-10 h-10 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path>
							</svg>
						</div>
					</div>
					
					<h2 class="text-2xl md:text-3xl font-display font-bold text-white mb-4">
						Email Support
					</h2>
					
					<p class="text-gray-400 text-lg leading-relaxed mb-8 max-w-2xl mx-auto">
						If you have any questions or need assistance, please contact us at
					</p>
					
					{#if !emailRevealed}
						<button on:click={revealEmail} class="reveal-button">
							<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path>
								<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"></path>
							</svg>
							Click to Reveal Email
						</button>
					{:else}
						<div class="fade-in">
							<a href="mailto:support@digipulation.com" class="email-link text-xl">
								support@digipulation.com
							</a>
						</div>
					{/if}
				</div>
			</div>
		</div>
	</main>
</div>