<script>
	import { page } from '$app/stores';
	
	// Get error details from the page store
	let status = $derived($page.status);
	let message = $derived($page.error?.message || 'Something went wrong');
	
	// Dynamic error messages based on status code
	let errorTitle = $derived(() => {
		switch (status) {
			case 404: return "Round Lost: Page Not Found";
			case 403: return "Access Denied";
			case 500: return "Server Meltdown";
			default: return "Oops!";
		}
	});
	
	let errorDescription = $derived(() => {
		switch (status) {
			case 404: return "You whiffed the entry — this CS2 giveaways page doesn't exist. Use the links below to rotate back to free cases, promo codes and top sites.";
			case 403: return "You don't have permission to access this area.";
			case 500: return "Our servers are having a moment. Please try again.";
			default: return message;
		}
	});
</script>

<div class="error-page">
	<!-- Animated Background -->
	<div class="background-effects">
		<div class="orb orb-1"></div>
		<div class="orb orb-2"></div>
		<div class="grid-overlay"></div>
		<div class="scanline"></div>
	</div>
	
	<!-- Floating Particles -->
	<div class="particles">
		{#each Array(40) as _, i}
			<div
				class="particle"
				style="
					--delay: {Math.random() * 6}s;
					--x: {Math.random() * 100}%;
					--duration: {4 + Math.random() * 6}s;
					--drift: {Math.random() * 40 - 20}px;
					--size: {2 + Math.random() * 4}px;
				"
			></div>
		{/each}
	</div>
	
	<!-- Main Content -->
	<div class="content">
		<!-- Glitch Error Code -->
		<div class="error-code-wrapper">
			<span class="error-code" data-text={status}>{status}</span>
			<div class="error-code-shadow">{status}</div>
		</div>
		
		<!-- Error Title -->
		<h1 class="error-title">
			<span class="title-accent">{">"}</span>
			{errorTitle()}
			<span class="cursor">_</span>
		</h1>
		
		<!-- Error Description -->
		<p class="error-description">{errorDescription()}</p>
		
		<!-- Decorative Divider -->
		<div class="divider">
			<div class="divider-line"></div>
			<div class="divider-icon">
				<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon>
				</svg>
			</div>
			<div class="divider-line"></div>
		</div>
		
		<!-- Action Buttons -->
		<div class="actions">
			<a href="/" class="btn btn-primary">
				<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
					<polyline points="9 22 9 12 15 12 15 22"></polyline>
				</svg>
				Return Home
			</a>
			<button onclick={() => history.back()} class="btn btn-secondary">
				<svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
					<line x1="19" y1="12" x2="5" y2="12"></line>
					<polyline points="12 19 5 12 12 5"></polyline>
				</svg>
				Go Back
			</button>
		</div>
		
		<!-- Quick Links -->
		<div class="quick-links">
			<span class="quick-links-label">Quick Links:</span>
			<a href="/promos">Promo Codes</a>
			<a href="/cases">Free Cases</a>
			<a href="/sites">Top Sites</a>
		</div>
	</div>
	
	<!-- Corner Decorations -->
	<div class="corner corner-tl"></div>
	<div class="corner corner-tr"></div>
	<div class="corner corner-bl"></div>
	<div class="corner corner-br"></div>
</div>

<style>
	.error-page {
		position: relative;
		min-height: calc(100vh - 200px);
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 2rem;
		overflow: hidden;
	}
	
	/* Background Effects */
	.background-effects {
		position: absolute;
		inset: 0;
		pointer-events: none;
		z-index: 0;
	}
	
	.orb {
		position: absolute;
		border-radius: 50%;
		filter: blur(80px);
		opacity: 0.4;
		animation: float 8s ease-in-out infinite;
	}
	
	.orb-1 {
		width: 400px;
		height: 400px;
		background: radial-gradient(circle, #FBAC18 0%, transparent 70%);
		top: -100px;
		right: -100px;
		animation-delay: 0s;
	}
	
	.orb-2 {
		width: 500px;
		height: 500px;
		background: radial-gradient(circle, #28397F 0%, transparent 70%);
		bottom: -150px;
		left: -150px;
		animation-delay: -4s;
	}
	
	.grid-overlay {
		position: absolute;
		inset: 0;
		background-image: 
			linear-gradient(rgba(251, 172, 24, 0.03) 1px, transparent 1px),
			linear-gradient(90deg, rgba(251, 172, 24, 0.03) 1px, transparent 1px);
		background-size: 50px 50px;
		mask-image: radial-gradient(ellipse at center, black 0%, transparent 70%);
	}
	
	.scanline {
		position: absolute;
		inset: 0;
		background: linear-gradient(
			transparent 50%,
			rgba(40, 57, 127, 0.02) 50%
		);
		background-size: 100% 4px;
		animation: scanlines 0.5s linear infinite;
	}
	
	/* Particles */
	.particles {
		position: absolute;
		inset: 0;
		pointer-events: none;
		overflow: hidden;
	}
	
	.particle {
		position: absolute;
		width: var(--size, 4px);
		height: var(--size, 4px);
		background: #FBAC18;
		border-radius: 50%;
		left: var(--x);
		bottom: -10px;
		opacity: 0;
		animation: particle-path var(--duration) ease-in-out infinite;
		animation-delay: var(--delay);
		box-shadow: 0 0 10px #FBAC18, 0 0 20px #FBAC18;
	}
	
	/* Content */
	.content {
		position: relative;
		z-index: 10;
		text-align: center;
		max-width: 600px;
	}
	
	/* Error Code */
	.error-code-wrapper {
		position: relative;
		margin-bottom: 1.5rem;
	}
	
	.error-code {
		font-family: 'Chakra Petch', sans-serif;
		font-size: clamp(8rem, 25vw, 14rem);
		font-weight: 700;
		line-height: 1;
		background: linear-gradient(135deg, #FBAC18 0%, #ffd700 50%, #FBAC18 100%);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		position: relative;
		animation: glitch 3s infinite;
		text-shadow: none;
	}
	
	.error-code::before,
	.error-code::after {
		content: attr(data-text);
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: linear-gradient(135deg, #FBAC18 0%, #ffd700 50%, #FBAC18 100%);
		background-clip: text;
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
	}
	
	.error-code::before {
		animation: glitch-1 0.3s infinite;
		clip-path: polygon(0 0, 100% 0, 100% 35%, 0 35%);
	}
	
	.error-code::after {
		animation: glitch-2 0.3s infinite;
		clip-path: polygon(0 65%, 100% 65%, 100% 100%, 0 100%);
	}
	
	.error-code-shadow {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		font-family: 'Chakra Petch', sans-serif;
		font-size: clamp(8rem, 25vw, 14rem);
		font-weight: 700;
		color: #28397F;
		opacity: 0.15;
		filter: blur(20px);
		z-index: -1;
	}
	
	/* Error Title */
	.error-title {
		font-family: 'Chakra Petch', sans-serif;
		font-size: clamp(1.5rem, 5vw, 2.5rem);
		font-weight: 700;
		color: white;
		margin-bottom: 1rem;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.5rem;
	}
	
	.title-accent {
		color: #FBAC18;
		font-weight: 400;
	}
	
	.cursor {
		color: #FBAC18;
		animation: blink 1s step-end infinite;
	}
	
	/* Error Description */
	.error-description {
		font-size: 1.1rem;
		color: #9ca3af;
		margin-bottom: 2rem;
		line-height: 1.6;
	}
	
	/* Divider */
	.divider {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 1rem;
		margin-bottom: 2rem;
	}
	
	.divider-line {
		width: 60px;
		height: 2px;
		background: linear-gradient(90deg, transparent, #28397F, transparent);
	}
	
	.divider-icon {
		color: #FBAC18;
		animation: pulse 2s ease-in-out infinite;
	}
	
	/* Action Buttons */
	.actions {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
		margin-bottom: 2rem;
	}
	
	.btn {
		display: inline-flex;
		align-items: center;
		gap: 0.5rem;
		padding: 0.875rem 1.75rem;
		font-size: 0.95rem;
		font-weight: 600;
		border-radius: 8px;
		text-decoration: none;
		transition: all 0.3s ease;
		cursor: pointer;
		border: none;
	}
	
	.btn-primary {
		background: linear-gradient(135deg, #FBAC18 0%, #e09800 100%);
		color: #0f1115;
		box-shadow: 0 4px 20px rgba(251, 172, 24, 0.3);
	}
	
	.btn-primary:hover {
		transform: translateY(-2px);
		box-shadow: 0 6px 30px rgba(251, 172, 24, 0.5);
	}
	
	.btn-secondary {
		background: rgba(40, 57, 127, 0.2);
		color: white;
		border: 1px solid rgba(40, 57, 127, 0.5);
	}
	
	.btn-secondary:hover {
		background: rgba(40, 57, 127, 0.4);
		border-color: #28397F;
		transform: translateY(-2px);
	}
	
	/* Quick Links */
	.quick-links {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.75rem;
		flex-wrap: wrap;
		font-size: 0.875rem;
	}
	
	.quick-links-label {
		color: #6b7280;
	}
	
	.quick-links a {
		color: #FBAC18;
		text-decoration: none;
		transition: all 0.2s ease;
		padding: 0.25rem 0.5rem;
		border-radius: 4px;
	}
	
	.quick-links a:hover {
		color: white;
		background: rgba(251, 172, 24, 0.1);
	}
	
	/* Corner Decorations */
	.corner {
		position: absolute;
		width: 80px;
		height: 80px;
		border: 2px solid rgba(40, 57, 127, 0.3);
		pointer-events: none;
	}
	
	.corner-tl {
		top: 2rem;
		left: 2rem;
		border-right: none;
		border-bottom: none;
	}
	
	.corner-tr {
		top: 2rem;
		right: 2rem;
		border-left: none;
		border-bottom: none;
	}
	
	.corner-bl {
		bottom: 2rem;
		left: 2rem;
		border-right: none;
		border-top: none;
	}
	
	.corner-br {
		bottom: 2rem;
		right: 2rem;
		border-left: none;
		border-top: none;
	}
	
	/* Animations */
	@keyframes float {
		0%, 100% { transform: translate(0, 0) scale(1); }
		50% { transform: translate(30px, -30px) scale(1.1); }
	}
	
	@keyframes scanlines {
		0% { background-position: 0 0; }
		100% { background-position: 0 4px; }
	}
	
	@keyframes particle-path {
		0% {
			opacity: 0;
			transform: translate3d(0, 0, 0) scale(0.3);
		}
		10% {
			opacity: 1;
		}
		40% {
			transform: translate3d(calc(var(--drift) * 0.3), -40vh, 0) scale(0.85);
		}
		70% {
			transform: translate3d(calc(var(--drift) * 0.7), -75vh, 0) scale(1);
			opacity: 1;
		}
		100% {
			opacity: 0;
			transform: translate3d(var(--drift), -110vh, 0) scale(1.05);
		}
	}
	
	@keyframes glitch {
		0%, 90%, 100% { transform: translate(0); }
		92% { transform: translate(-2px, 2px); }
		94% { transform: translate(2px, -2px); }
		96% { transform: translate(-2px, -2px); }
		98% { transform: translate(2px, 2px); }
	}
	
	@keyframes glitch-1 {
		0%, 100% { transform: translate(0); }
		20% { transform: translate(-3px, 3px); }
		40% { transform: translate(3px, -3px); }
		60% { transform: translate(-3px, -3px); }
		80% { transform: translate(3px, 3px); }
	}
	
	@keyframes glitch-2 {
		0%, 100% { transform: translate(0); }
		20% { transform: translate(3px, -3px); }
		40% { transform: translate(-3px, 3px); }
		60% { transform: translate(3px, 3px); }
		80% { transform: translate(-3px, -3px); }
	}
	
	@keyframes blink {
		0%, 100% { opacity: 1; }
		50% { opacity: 0; }
	}
	
	@keyframes pulse {
		0%, 100% { transform: scale(1); opacity: 1; }
		50% { transform: scale(1.1); opacity: 0.8; }
	}
	
	/* Responsive */
	@media (max-width: 640px) {
		.corner {
			width: 40px;
			height: 40px;
		}
		
		.corner-tl, .corner-tr, .corner-bl, .corner-br {
			top: 1rem;
			left: 1rem;
			right: 1rem;
			bottom: 1rem;
		}
		
		.corner-tr { left: auto; }
		.corner-bl { top: auto; }
		.corner-br { top: auto; left: auto; }
		
		.actions {
			flex-direction: column;
			align-items: center;
		}
		
		.btn {
			width: 100%;
			max-width: 250px;
			justify-content: center;
		}
	}
</style>