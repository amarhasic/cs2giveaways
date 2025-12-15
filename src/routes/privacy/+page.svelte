<script>
    import { page } from '$app/stores';
</script>

<svelte:head>
    <title>CS2 Giveaways - Privacy Policy</title>
    <link rel="icon" href="/favicon.png" />
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
</svelte:head>

<style>
    /* Custom styles for glow and animation */
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
        <div class="absolute inset-0 overflow-hidden pointer-events-none">
            <div class="absolute top-0 left-1/4 w-96 h-96 bg-yellow-500/5 rounded-full blur-3xl"></div>
            <div class="absolute bottom-0 right-1/4 w-96 h-96 bg-yellow-500/5 rounded-full blur-3xl"></div>
        </div>
        
        <div class="container mx-auto max-w-5xl px-4 py-12 relative z-10">
            <div class="text-center mb-16 fade-in-up">
                <h1 class="text-5xl md:text-6xl font-display font-bold text-white mb-4 header-glow">
                    Privacy <span class="gradient-text">Policy</span>
                </h1>
                <p class="text-gray-400 text-lg max-w-2xl mx-auto">
                    Please review our Privacy Policy for more information regarding data handling.
                </p>
            </div>
            
            <div class="text-center" style="margin-top: 100px;">
                <a href="https://www.digipulation.com/privacypolicy" class="px-5 py-2 bg-yellow-500 hover:bg-yellow-400 text-black font-bold text-sm rounded shadow-[0_0_15px_rgba(234,179,8,0.2)] hover:shadow-[0_0_20px_rgba(234,179,8,0.4)] transition-all transform hover:-translate-y-0.5 active:translate-y-0">
                    View Privacy Policy
                </a>
            </div>
        </div>
    </main>
</div>