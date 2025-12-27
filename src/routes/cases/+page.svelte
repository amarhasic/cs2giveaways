<script>
	// Partner site data with brand colors and affiliate bonuses
	const partners = [
		{
			id: 1,
			name: 'CSGOEmpire',
			logo: '/csgoempire.jpg',
			caseImage: '/csgoempire.jpg',
			brandColor: '#FBAC18',
			affiliateBonus: 'FREE: 1 Case',
			url: 'https://csgoempire.com/r/digipulation',
			code: 'digipulation'
		},
		{
			id: 2,
			name: 'Key-Drop',
			logo: '/keydrop.jpg',
			caseImage: '/keydrop.jpg',
			brandColor: '#28397F',
			affiliateBonus: 'FREE: 1 Case',
			url: 'https://kd.link/?code=DIGIPULATION',
			code: 'DIGIPULATION'
		},
		{
			id: 3,
			name: 'Howl.gg',
			logo: '/howl.jpg',
			caseImage: '/howl.jpg',
			brandColor: '#FBAC18',
			affiliateBonus: 'FREE: 1 Case',
			url: 'https://howl.gg/r/dpl',
			code: 'dpl'
		},
		{
			id: 4,
			name: 'Clash.gg',
			logo: '/clash.jpg',
			caseImage: '/clash.jpg',
			brandColor: '#28397F',
			affiliateBonus: 'FREE: 3 Cases',
			url: 'https://clash.gg/r/DIGIPULATION',
			code: 'DIGIPULATION'
		},
		{
			id: 5,
			name: 'DatDrop',
			logo: '/datdrop.jpg',
			caseImage: '/datdrop.jpg',
			brandColor: '#FBAC18',
			affiliateBonus: 'DAILY: Free Case',
			url: 'https://datdrop.com/p/digipulation',
			code: 'digipulation'
		},
		{
			id: 6,
			name: 'Farmskins',
			logo: '/farmskins.jpg',
			caseImage: '/farmskins.jpg',
			brandColor: '#28397F',
			affiliateBonus: 'DAILY: Free Cases',
			url: 'https://farmskins.com/ref-digipulation',
			code: 'digipulation'
		}
	];

	// State for hover effects
	let hoveredCard = $state(null);

	// Handle card click - copy code and open site
	async function handleCardClick(partner) {
		try {
			await navigator.clipboard.writeText(partner.code);
			window.open(partner.url, '_blank', 'noopener,noreferrer');
		} catch (err) {
			console.error('Failed to copy code:', err);
			window.open(partner.url, '_blank', 'noopener,noreferrer');
		}
	}

	// SEO Data
	const schemaOrg = {
		'@context': 'https://schema.org',
		'@type': 'ItemList',
		itemListElement: partners.map((partner, index) => ({
			'@type': 'ListItem',
			position: index + 1,
			url: `https://cs2giveaways.com/cases#${partner.name.toLowerCase().replace(/\s+/g, '-')}`,
			name: `Free ${partner.name} Case`,
			description: `Claim free ${partner.name} cases with code ${partner.code}. ${partner.affiliateBonus}.`
		}))
	};
</script>

<svelte:head>
	<title>Free CS2 Cases 2025 | Claim Daily Free Cases & Skins</title>
	<meta
		name="description"
		content="Open free CS2 cases daily! Verified list of sites offering free cases, no deposit needed. Use our codes to claim free skins from Empire, Key-Drop & more."
	/>
	<meta
		name="keywords"
		content="free cs2 cases, csgo cases free, key-drop free case, csgoempire free case, daily free cases, free skins no deposit"
	/>

	<!-- JSON-LD Structured Data -->
	{@html `<script type="application/ld+json">${JSON.stringify(schemaOrg)}</script>`}
</svelte:head>

<div class="gateway-container">
	<!-- Header -->
	<header class="gateway-header">
		<span class="header-badge">PARTNER GATEWAYS</span>
		<h1 class="header-title">Trusted <span class="accent">Partners</span></h1>
		<p class="header-subtitle">Click any gateway to copy code & visit site</p>
	</header>

	<!-- Gateway Grid -->
	<section class="gateway-grid">
		{#each partners as partner, index}
			<div
				class="gateway-card"
				class:is-hovered={hoveredCard === index}
				style="--brand-color: {partner.brandColor}"
				role="button"
				tabindex="0"
				onmouseenter={() => (hoveredCard = index)}
				onmouseleave={() => (hoveredCard = null)}
				onclick={() => handleCardClick(partner)}
				onkeydown={(e) => (e.key === 'Enter' || e.key === ' ') && handleCardClick(partner)}
			>
				<!-- Site Identity Badge -->
				<div class="site-badge">
					<img src={partner.logo} alt="{partner.name} logo" class="site-logo" />
					<h2 class="site-name" style="font-size: 1.1rem; margin: 0;">{partner.name}</h2>
				</div>

				<!-- Case Spotlight -->
				<div class="case-spotlight">
					<img src={partner.caseImage} alt="{partner.name} case" class="case-image" />

					<!-- Hover Action Button -->
					<div class="hover-action">
						<button class="action-button" tabindex="-1">
							<svg
								class="copy-icon"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
							>
								<rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect>
								<path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"></path>
							</svg>
							COPY CODE & VISIT
						</button>
					</div>
				</div>

				<!-- Free Value Proposition -->
				<div class="value-proposition">
					<span class="bonus-text">{partner.affiliateBonus}</span>
					<span class="code-hint">Code: {partner.code}</span>
				</div>
			</div>
		{/each}
	</section>

	<!-- Footer -->
	<footer class="gateway-footer">
		<p class="footer-text">All partner sites are verified and trusted</p>
		<a href="/freeskinspath" class="footer-link"> View Complete Free Skins Path → </a>
	</footer>
</div>

<style>
	/* ===== CONTAINER ===== */
	.gateway-container {
		min-height: 100vh;
		background: #000000;
		padding: 2rem 1rem;
	}

	/* ===== HEADER ===== */
	.gateway-header {
		text-align: center;
		margin-bottom: 3rem;
		padding-top: 2rem;
	}

	.header-badge {
		display: inline-block;
		font-size: 0.7rem;
		font-weight: 600;
		letter-spacing: 0.3em;
		color: #fbac18;
		padding: 0.5rem 1rem;
		background: rgba(251, 172, 24, 0.1);
		border: 1px solid rgba(251, 172, 24, 0.3);
		border-radius: 2rem;
		margin-bottom: 1rem;
	}

	.header-title {
		font-size: clamp(2rem, 6vw, 3.5rem);
		font-weight: 700;
		color: #fff;
		margin-bottom: 0.5rem;
	}

	.accent {
		color: #fbac18;
	}

	.header-subtitle {
		font-size: 1rem;
		color: rgba(255, 255, 255, 0.6);
	}

	/* ===== GATEWAY GRID ===== */
	.gateway-grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
		gap: 2rem;
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 1rem;
	}

	/* ===== GATEWAY CARD ===== */
	.gateway-card {
		position: relative;
		background: rgba(255, 255, 255, 0.05);
		border: 2px solid rgba(255, 255, 255, 0.1);
		border-radius: 1rem;
		padding: 1.5rem;
		cursor: pointer;
		transition: all 0.3s ease;
		overflow: hidden;
	}

	.gateway-card:hover {
		transform: translateY(-4px);
		border-color: var(--brand-color);
		box-shadow:
			0 8px 32px rgba(0, 0, 0, 0.3),
			0 0 20px var(--brand-color);
	}

	/* ===== SITE IDENTITY BADGE ===== */
	.site-badge {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		margin-bottom: 1.5rem;
	}

	.site-logo {
		width: 40px;
		height: 40px;
		border-radius: 0.5rem;
		object-fit: cover;
		border: 2px solid rgba(255, 255, 255, 0.2);
	}

	.site-name {
		font-size: 1.1rem;
		font-weight: 600;
		color: #fff;
	}

	/* ===== CASE SPOTLIGHT ===== */
	.case-spotlight {
		position: relative;
		width: 100%;
		height: 200px;
		background: #000000;
		border-radius: 0.75rem;
		overflow: hidden;
		margin-bottom: 1.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.case-image {
		width: 120px;
		height: 120px;
		object-fit: cover;
		border-radius: 0.5rem;
		transition: transform 0.3s ease;
	}

	.gateway-card:hover .case-image {
		transform: scale(1.05);
	}

	/* ===== HOVER ACTION ===== */
	.hover-action {
		position: absolute;
		inset: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		background: rgba(0, 0, 0, 0.8);
		opacity: 0;
		transition: opacity 0.3s ease;
	}

	.gateway-card.is-hovered .hover-action {
		opacity: 1;
	}

	.action-button {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		background: var(--brand-color);
		color: #fff;
		border: none;
		padding: 0.75rem 1.5rem;
		border-radius: 0.5rem;
		font-weight: 600;
		font-size: 0.9rem;
		cursor: pointer;
		transition: transform 0.2s ease;
	}

	.action-button:hover {
		transform: scale(1.05);
	}

	.copy-icon {
		width: 18px;
		height: 18px;
	}

	/* ===== VALUE PROPOSITION ===== */
	.value-proposition {
		text-align: center;
	}

	.bonus-text {
		display: block;
		font-size: 1.2rem;
		font-weight: 700;
		color: #fbac18;
		text-shadow: 0 0 10px rgba(251, 172, 24, 0.5);
		margin-bottom: 0.5rem;
	}

	.code-hint {
		font-size: 0.8rem;
		color: rgba(255, 255, 255, 0.5);
		font-family: 'Courier New', monospace;
	}

	/* ===== FOOTER ===== */
	.gateway-footer {
		text-align: center;
		margin-top: 4rem;
		padding-bottom: 2rem;
	}

	.footer-text {
		font-size: 0.85rem;
		color: rgba(255, 255, 255, 0.4);
		margin-bottom: 1rem;
	}

	.footer-link {
		display: inline-block;
		font-size: 0.9rem;
		font-weight: 600;
		color: #fbac18;
		text-decoration: none;
		padding: 0.75rem 1.5rem;
		border: 1px solid rgba(251, 172, 24, 0.3);
		border-radius: 0.5rem;
		transition: all 0.3s ease;
	}

	.footer-link:hover {
		background: rgba(251, 172, 24, 0.1);
		border-color: #fbac18;
	}

	/* ===== RESPONSIVE ===== */
	@media (max-width: 640px) {
		.gateway-grid {
			grid-template-columns: 1fr;
			gap: 1.5rem;
		}

		.gateway-card {
			padding: 1.25rem;
		}

		.case-spotlight {
			height: 160px;
		}

		.case-image {
			width: 100px;
			height: 100px;
		}
	}
</style>
