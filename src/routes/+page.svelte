<script>
    import { onMount } from 'svelte';

    // Menu data using Svelte 5 runes
    let menuData = $state({
        meals: [
            {
                name: "Smash Burger With Fries",
                description: "Grilled Onions, American Cheese, Muzzy's Sauce",
                price: 10.99
            },
            {
                name: "Bacon Smash Burger With Fries",
                description: "Grilled Onions, American Cheese, Pickles, Muzzy's Sauce",
                price: 11.99
            },
            {
                name: "Chicken Slider With Fries",
                description: "Kale Slaw, Pickles, American Cheese, Muzzy's Sauce",
                price: 9.99
            },
            {
                name: "2x Tenders With Fries",
                description: "Slice of Bread, Pickles, Side of Muzzy's Sauce",
                price: 12.99
            },
            {
                name: "2x Chicken Slider With Fries",
                description: "Kale Slaw, Pickles, American Cheese, Muzzy's Sauce",
                price: 14.99
            },
            {
                name: "1x Chicken Slider & 1x Tender With Fries",
                description: "Kale Slaw, Pickles, American Cheese, Slice of Bread, Pickles, Side of Sauce",
                price: 13.99
            },
            {
                name: "Loaded Fries",
                description: "",
                price: 13.99
            }
        ],
        sides: [
            { name: "Smash Burger", price: 6.99 },
            { name: "Chicken Slider", price: 6.99 },
            { name: "Fries", price: 4.5 },
            { name: "Cheese Fries", price: 5.5 },
            { name: "Mac & Cheese", price: 4.99 },
            { name: "Kale Slaw", price: 4.99 },
            { name: "Tender", price: 4.99 },
            { name: "Cup of Pickles", price: 2.5 },
            { name: "Large Muzzy's Sauce", price: 3.5 },
            { name: "Side of Muzzy's Sauce", price: 0.38 },
            { name: "Side of Cheese Sauce", price: 0.59 }
        ],
        fountain_drinks: [
            { name: "Ice Tea", price: 3.2 },
            { name: "Pink Lemonade", price: 3.2 },
            { name: "Lemonade", price: 3.2 },
            { name: "Half & Half Drink", price: 3.4 }
        ],
        bottle_drinks: [
            { name: "Water", price: 1.99 },
            { name: "Mexican Coke", price: 3.5 },
            { name: "Mexican Sprite", price: 3.5 },
            { name: "Mexican Fanta", price: 3.5 },
            { name: "20 oz. Coke", price: 2.7 },
            { name: "20 oz. Diet Coke", price: 2.7 },
            { name: "20 oz. Sprite", price: 2.7 },
            { name: "20 oz. Dr Pepper", price: 2.7 },
            { name: "20 oz. Diet Dr. Pepper", price: 2.7 }
        ],
        milkshakes: [
            { name: "M&Ms Milkshake", price: 5.99 },
            { name: "Oreo Milkshake", price: 5.99 },
            { name: "Strawberry Milkshake", price: 5.99 },
            { name: "Chocolate Milkshake", price: 5.99 },
            { name: "Vanilla Milkshake", price: 5.99 }
        ],
        desserts_ice_cream: [
            { name: "Chocolate Ice Cream", price: 5.99 },
            { name: "Cookies & Cream Ice Cream", price: 5.99 },
            { name: "Strawberry Ice Cream", price: 5.99 },
            { name: "Vanilla Ice Cream", price: 5.99 },
            { name: "Banana Pudding", price: 5.99 }
        ]
    });

    // Section configuration
    const sectionConfig = $state({
        meals: { title: 'Entrées', emoji: '🔥' },
        sides: { title: 'Appetizers', emoji: '🌶️' },
        fountain_drinks: { title: 'Fountain Drinks', emoji: '🥤' },
        milkshakes: { title: 'Milkshakes', emoji: '🥛' },
        bottle_drinks: { title: 'Bottled Bevs', emoji: '🍾' },
        desserts_ice_cream: { title: 'Desserts & Ice Cream', emoji: '🍨' }
    });

    let cart = $state([]);
    let isCartOverlayOpen = $state(false);

    // Hover states using runes
    let hoveredSection = $state(null);
    let activeSection = $state('Entrées');
    let sparkles = $state([]);

    // Functions for animations
    function handleSectionHover(sectionKey) {
        hoveredSection = sectionKey;
    }

    function handleSectionLeave() {
        hoveredSection = null;
    }

    function createSparkle(event) {
        const sparkle = {
            id: Date.now() + Math.random(),
            x: event.clientX,
            y: event.clientY
        };

        sparkles = [...sparkles, sparkle];

        setTimeout(() => {
            sparkles = sparkles.filter(s => s.id !== sparkle.id);
        }, 1000);
    }

    function scrollToSection(sectionTitle) {
        const element = document.getElementById(`${sectionTitle}-section`);
        if (element) {
            element.scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }
    }

    function addToCart(item) {
        const existingIndex = cart.findIndex(cartItem => cartItem.name === item.name);

        if (existingIndex >= 0) {
            // Item already exists, increase quantity
            updateQuantity(existingIndex, 1);
        } else {
            // New item, add to cart
            cart = [...cart, { ...item, quantity: 1 }];
        }
    }

    function updateQuantity(index, change) {
        cart = cart.map((item, i) => {
            if (i === index) {
                const newQuantity = (item.quantity || 1) + change;

                // Remove item if quantity would be 0 or less
                if (newQuantity <= 0) {
                    return null;
                }

                // Update the quantity
                return {
                    ...item,
                    quantity: newQuantity
                };
            }
            return item;
        }).filter(item => item !== null); // Remove null items
    }

    function removeFromCart(index) {
        cart = cart.filter((_, i) => i !== index);
    }

    function clearCart() {
        cart = [];
    }

    function toggleCartOverlay() {
        isCartOverlayOpen = !isCartOverlayOpen;
    }

    function closeCartOverlay() {
        isCartOverlayOpen = false;
    }

    // Reactive computed values using $derived
    const menuSections = $derived(Object.entries(menuData));
    const cartItemCount = $derived(cart.reduce((total, item) => total + (item.quantity || 1), 0));
    const cartTotal = $derived(cart.reduce((total, item) => total + ((item.price || 0) * (item.quantity || 1)), 0));

    onMount(() => {
        const observers = new Map();

        menuSections.forEach(([key]) => {
            const config = sectionConfig[key];
            if (!config) return;

            const targetId = `${config.title}-section`;
            const target = document.getElementById(targetId);

            if (!target) return;

            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        activeSection = config.title;

                        // Add entrance animation
                        entry.target.style.animation = 'slideInFromRight 0.6s ease-out';
                        setTimeout(() => {
                            entry.target.style.animation = '';
                        }, 600);
                    }
                });
            }, {
                rootMargin: '-20% 0px -50% 0px',
                threshold: 0.1
            });

            observer.observe(target);
            observers.set(targetId, observer);
        });

        // Cleanup observers on destroy
        return () => {
            observers.forEach(observer => observer.disconnect());
        };
    });
</script>

<!-- Header -->
<header class="hero-gradient text-white py-16 relative overflow-hidden">
    <div class="absolute inset-0 opacity-20">
        <div class="absolute top-10 left-10 w-32 h-32 bg-orange-500 rounded-full blur-xl float-animation"></div>
        <div class="absolute top-32 right-20 w-24 h-24 bg-red-500 rounded-full blur-xl float-animation delay-2"></div>
        <div class="absolute bottom-20 left-1/3 w-20 h-20 bg-yellow-500 rounded-full blur-xl float-animation delay-4"></div>
    </div>
    <div class="container mx-auto px-6 text-center relative z-10">
        <h1 class="text-4xl md:text-7xl font-black mb-4 tracking-tight text-transparent bg-gradient-to-r from-orange-400 via-red-500 to-yellow-500 bg-clip-text">
            Muzzy's Shack
        </h1>
        <p class="text-lg md:text-xl text-gray-200 max-w-2xl mx-auto leading-relaxed font-medium">
            🔥 Blazing Hot Chicken & More 🔥
        </p>
        <div class="mt-8 inline-block px-6 md:px-8 py-2 md:py-3 bg-gradient-to-r from-orange-600 to-red-600 backdrop-blur-sm rounded-full text-xs md:text-sm font-bold shadow-lg">
            Scroll to explore our fiery menu
        </div>
    </div>
</header>

<!-- Desktop Cart (Hidden on Mobile) -->
{#if cart.length > 0}
    <div class="hidden lg:block fixed top-6 right-6 z-50 w-96 max-w-[calc(100vw-3rem)]">
        <!-- Cart Header -->
        <div class="bg-gradient-to-r from-orange-600 to-red-600 rounded-t-2xl p-4 shadow-lg animate-pulse">
            <div class="flex items-center justify-between">
                <h3 class="text-white font-black text-lg flex items-center gap-2">
                    🛒 Your Order
                </h3>
                <div class="bg-white/20 backdrop-blur-sm rounded-full px-3 py-1">
                    <span class="text-white font-bold text-sm">{cart.length} items</span>
                </div>
            </div>
        </div>

        <!-- Cart Items -->
        <div class="bg-gradient-to-b from-gray-900 to-black rounded-b-2xl border-x-2 border-b-2 border-orange-900/30 shadow-2xl max-h-96 overflow-y-auto">
            <div class="p-4 space-y-3">
                {#each cart as item, index}
                    <div class="bg-gray-800/60 backdrop-blur-sm rounded-xl p-4 border border-orange-900/20 hover:border-orange-500/40 transition-all duration-300 group hover:-translate-x-1 hover:shadow-lg hover:shadow-orange-500/20">
                        <div class="flex justify-between items-start">
                            <div class="flex-1">
                                <h4 class="text-white font-bold text-sm mb-1 group-hover:text-orange-300 transition-colors duration-300">
                                    {item.name}
                                </h4>
                                {#if item.description}
                                    <p class="text-gray-400 text-xs mb-2 overflow-hidden h-8">
                                        {item.description}
                                    </p>
                                {/if}

                                <!-- Quantity Controls -->
                                <div class="flex items-center gap-3 mt-2">
                                    <div class="flex items-center bg-gray-700/50 rounded-lg border border-orange-900/30">
                                        <button
                                                class="px-3 py-1 text-orange-400 hover:text-orange-300 hover:bg-orange-900/20 rounded-l-lg transition-all duration-200 hover:scale-110"
                                                onclick={() => updateQuantity(index, -1)}
                                        >
                                            −
                                        </button>
                                        <span class="px-3 py-1 text-white font-bold text-sm bg-gray-800/50 border-x border-orange-900/30">
                                            {item.quantity || 1}
                                        </span>
                                        <button
                                                class="px-3 py-1 text-orange-400 hover:text-orange-300 hover:bg-orange-900/20 rounded-r-lg transition-all duration-200 hover:scale-110"
                                                onclick={() => updateQuantity(index, 1)}
                                        >
                                            +
                                        </button>
                                    </div>

                                    <!-- Remove Item -->
                                    <button
                                            class="p-2 text-red-400 hover:text-red-300 hover:bg-red-900/20 rounded-lg transition-all duration-200 hover:scale-110"
                                            onclick={() => removeFromCart(index)}
                                            title="Remove item"
                                    >
                                        🗑️
                                    </button>
                                </div>
                            </div>

                            <!-- Price -->
                            <div class="ml-4 text-right">
                                <div class="bg-gradient-to-r from-orange-600 to-red-600 text-white px-3 py-1 rounded-lg font-black text-sm shadow-lg group-hover:scale-105 group-hover:shadow-orange-500/40 transition-all duration-200">
                                    ${((item.price || 0) * (item.quantity || 1)).toFixed(2)}
                                </div>
                                {#if item.quantity > 1}
                                    <div class="text-gray-400 text-xs mt-1">
                                        ${item.price?.toFixed(2)} each
                                    </div>
                                {/if}
                            </div>
                        </div>
                    </div>
                {/each}
            </div>

            <!-- Cart Summary -->
            <div class="bg-gradient-to-r from-gray-800 to-gray-900 border-t border-orange-900/30 p-4">
                <!-- Subtotal -->
                <div class="flex justify-between items-center mb-3">
                    <span class="text-gray-300 font-medium">Subtotal:</span>
                    <span class="text-white font-bold text-lg">
                        ${cartTotal.toFixed(2)}
                    </span>
                </div>

                <!-- Tax -->
                <div class="flex justify-between items-center mb-3">
                    <span class="text-gray-400 text-sm">Tax (8.25%):</span>
                    <span class="text-gray-300 text-sm">
                        ${(cartTotal * 0.0825).toFixed(2)}
                    </span>
                </div>

                <!-- Total -->
                <div class="border-t border-orange-900/30 pt-3 mb-4">
                    <div class="flex justify-between items-center">
                        <span class="text-white font-black text-lg">Total:</span>
                        <span class="text-transparent bg-gradient-to-r from-orange-400 to-red-400 bg-clip-text font-black text-xl">
                            ${(cartTotal * 1.0825).toFixed(2)}
                        </span>
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="space-y-2">
                    <button class="w-full bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white font-black py-3 rounded-xl transition-all duration-300 shadow-lg hover:shadow-xl hover:scale-105 hover:shadow-orange-500/40">
                        🔥 Checkout Now
                    </button>
                    <button
                            class="w-full bg-gray-700/50 hover:bg-gray-600/50 text-gray-300 hover:text-white font-medium py-2 rounded-xl transition-all duration-300 border border-gray-600/30"
                            onclick={() => clearCart()}
                    >
                        Clear Cart
                    </button>
                </div>
            </div>
        </div>
    </div>
{/if}

<!-- Mobile Cart Overlay -->
{#if isCartOverlayOpen}
    <div class="lg:hidden fixed inset-0 z-50 bg-black/90 backdrop-blur-sm" onclick={closeCartOverlay}>
        <div class="h-full flex flex-col" onclick={(e) => e.stopPropagation()}>
            <!-- Cart Header -->
            <div class="bg-gradient-to-r from-orange-600 to-red-600 p-6 flex items-center justify-between">
                <div>
                    <h2 class="text-white font-black text-2xl">🛒 Your Order</h2>
                    <p class="text-orange-100 text-sm">{cartItemCount} items • ${cartTotal.toFixed(2)}</p>
                </div>
                <button
                        class="text-white p-2 hover:bg-white/20 rounded-xl transition-all duration-200"
                        onclick={closeCartOverlay}
                >
                    ✕
                </button>
            </div>

            <!-- Cart Items -->
            <div class="flex-1 overflow-y-auto bg-black p-4">
                {#if cart.length === 0}
                    <div class="text-center text-gray-400 mt-20">
                        <div class="text-6xl mb-4">🛒</div>
                        <p class="text-xl">Your cart is empty</p>
                        <p class="text-sm mt-2">Add some delicious items!</p>
                    </div>
                {:else}
                    <div class="space-y-4">
                        {#each cart as item, index}
                            <div class="bg-gray-900/50 backdrop-blur-sm rounded-2xl p-6 border border-orange-900/20">
                                <div class="flex justify-between items-start mb-4">
                                    <div class="flex-1">
                                        <h3 class="text-white font-bold text-lg mb-2">{item.name}</h3>
                                        {#if item.description}
                                            <p class="text-gray-400 text-sm mb-3">{item.description}</p>
                                        {/if}
                                    </div>
                                    <button
                                            class="p-2 text-red-400 hover:text-red-300 hover:bg-red-900/20 rounded-lg transition-all duration-200"
                                            onclick={() => removeFromCart(index)}
                                    >
                                        🗑️
                                    </button>
                                </div>

                                <div class="flex justify-between items-center">
                                    <!-- Quantity Controls -->
                                    <div class="flex items-center bg-gray-700/50 rounded-xl border border-orange-900/30">
                                        <button
                                                class="px-4 py-3 text-orange-400 hover:text-orange-300 hover:bg-orange-900/20 rounded-l-xl transition-all duration-200 text-xl"
                                                onclick={() => updateQuantity(index, -1)}
                                        >
                                            −
                                        </button>
                                        <span class="px-6 py-3 text-white font-bold text-lg bg-gray-800/50 border-x border-orange-900/30">
                                            {item.quantity || 1}
                                        </span>
                                        <button
                                                class="px-4 py-3 text-orange-400 hover:text-orange-300 hover:bg-orange-900/20 rounded-r-xl transition-all duration-200 text-xl"
                                                onclick={() => updateQuantity(index, 1)}
                                        >
                                            +
                                        </button>
                                    </div>

                                    <!-- Price -->
                                    <div class="text-right">
                                        <div class="bg-gradient-to-r from-orange-600 to-red-600 text-white px-4 py-2 rounded-xl font-black text-lg">
                                            ${((item.price || 0) * (item.quantity || 1)).toFixed(2)}
                                        </div>
                                        {#if item.quantity > 1}
                                            <div class="text-gray-400 text-sm mt-1">
                                                ${item.price?.toFixed(2)} each
                                            </div>
                                        {/if}
                                    </div>
                                </div>
                            </div>
                        {/each}
                    </div>
                {/if}
            </div>

            <!-- Cart Footer -->
            {#if cart.length > 0}
                <div class="bg-gradient-to-r from-gray-900 to-black p-6 border-t border-orange-900/30">
                    <!-- Totals -->
                    <div class="space-y-2 mb-6">
                        <div class="flex justify-between text-gray-300">
                            <span>Subtotal:</span>
                            <span>${cartTotal.toFixed(2)}</span>
                        </div>
                        <div class="flex justify-between text-gray-400 text-sm">
                            <span>Tax (8.25%):</span>
                            <span>${(cartTotal * 0.0825).toFixed(2)}</span>
                        </div>
                        <div class="border-t border-orange-900/30 pt-3 flex justify-between text-white font-bold text-xl">
                            <span>Total:</span>
                            <span class="text-transparent bg-gradient-to-r from-orange-400 to-red-400 bg-clip-text">
                                ${(cartTotal * 1.0825).toFixed(2)}
                            </span>
                        </div>
                    </div>

                    <!-- Action Buttons -->
                    <div class="space-y-3">
                        <button class="w-full bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white font-black py-4 rounded-2xl transition-all duration-300 shadow-lg text-lg">
                            🔥 Checkout Now
                        </button>
                        <button
                                class="w-full bg-gray-700/50 hover:bg-gray-600/50 text-gray-300 hover:text-white font-medium py-3 rounded-2xl transition-all duration-300 border border-gray-600/30"
                                onclick={() => clearCart()}
                        >
                            Clear Cart
                        </button>
                    </div>
                </div>
            {/if}
        </div>
    </div>
{/if}

<!-- Menu Container -->
<main class="container mx-auto px-4 md:px-6 py-12 flex bg-black min-h-screen">
    <!-- Desktop Sidebar Navigation (Hidden on Mobile) -->
    <nav class="hidden lg:block fixed left-6 top-1/2 transform -translate-y-1/2 z-40">
        <div class="bg-gradient-to-b from-gray-900 to-black rounded-2xl p-6 shadow-2xl border border-orange-900/30">
            <ul class="flex flex-col space-y-4">
                {#each menuSections as [sectionKey, items]}
                    {@const config = sectionConfig[sectionKey]}
                    <li>
                        <button
                                class="nav-item w-full text-left p-4 rounded-xl transition-all duration-300 font-bold text-sm relative overflow-hidden group"
                                class:active={activeSection === config.title}
                                onclick={() => scrollToSection(config.title)}
                        >
                            <span class="absolute inset-0 bg-gradient-to-r from-orange-600 to-red-600 opacity-0 group-hover:opacity-100 transition-opacity duration-300"></span>
                            <span class="relative z-10 flex items-center space-x-3">
                                <span class="text-lg">{config.emoji}</span>
                                <span class="text-white group-hover:text-white transition-colors duration-300">
                                    {config.title}
                                </span>
                            </span>
                        </button>
                    </li>
                {/each}
            </ul>
        </div>
    </nav>

    <!-- Mobile Bottom Navigation -->
    <nav class="lg:hidden fixed bottom-0 left-0 right-0 z-40 bg-gradient-to-r from-gray-900 to-black border-t border-orange-900/30 px-2 py-3">
        <div class="flex justify-between items-center max-w-sm mx-auto">
            {#each menuSections as [sectionKey, items]}
                {@const config = sectionConfig[sectionKey]}
                <button
                        class="nav-item-mobile flex flex-col items-center p-2 rounded-xl transition-all duration-300 relative group"
                        class:active={activeSection === config.title}
                        onclick={() => scrollToSection(config.title)}
                >
                    <span class="text-lg mb-1">{config.emoji}</span>
                    <span class="text-xs text-gray-400 group-hover:text-orange-300 transition-colors duration-300 text-center leading-tight">
                        {config.title.split(' ')[0]}
                    </span>
                </button>
            {/each}

            <!-- Mobile Cart Icon -->
            <button
                    class="nav-item-mobile flex flex-col items-center p-2 rounded-xl transition-all duration-300 relative group"
                    class:active={cart.length > 0}
                    onclick={toggleCartOverlay}
            >
                <div class="relative">
                    <span class="text-lg mb-1">🛒</span>
                    {#if cartItemCount > 0}
                        <span class="absolute -top-2 -right-2 bg-gradient-to-r from-orange-600 to-red-600 text-white text-xs font-bold rounded-full w-5 h-5 flex items-center justify-center">
                            {cartItemCount > 9 ? '9+' : cartItemCount}
                        </span>
                    {/if}
                </div>
                <span class="text-xs text-gray-400 group-hover:text-orange-300 transition-colors duration-300">
                    Cart
                </span>
            </button>
        </div>
    </nav>

    <!-- Menu Content -->
    <div class="lg:ml-64 w-full pb-20 lg:pb-0">
        {#each menuSections as [sectionKey, items]}
            {@const config = sectionConfig[sectionKey]}
            <div
                    class="menu-section bg-gradient-to-br from-gray-900 to-black rounded-3xl shadow-2xl p-6 md:p-8 mb-8 md:mb-12 relative overflow-hidden border border-orange-900/20"
                    class:hovered={hoveredSection === sectionKey}
                    onmouseenter={() => handleSectionHover(sectionKey)}
                    onmouseleave={handleSectionLeave}
                    onmousemove={createSparkle}
                    role="region"
                    aria-label={config.title}
                    id={config.title+'-section'}
            >
                <!-- Glow Effect -->
                <div class="absolute inset-0 bg-gradient-to-r from-orange-600/10 via-red-600/10 to-yellow-600/10 opacity-0 transition-opacity duration-500 hover:opacity-100"></div>

                <!-- Section Indicator -->
                <div
                        class="section-indicator absolute top-6 right-6 bg-gradient-to-r from-orange-600 to-red-600 text-white px-4 py-2 rounded-full text-sm font-bold shadow-lg"
                        class:visible={hoveredSection === sectionKey}
                >
                    {config.emoji} {config.title}
                </div>

                <!-- Section Header -->
                <div class="mb-6 md:mb-8 relative z-10">
                    <h2 class="text-2xl md:text-4xl font-black text-transparent bg-gradient-to-r from-orange-400 via-red-500 to-yellow-500 bg-clip-text mb-4">
                        {config.title}
                    </h2>
                    <div class="w-16 md:w-24 h-1 md:h-1.5 bg-gradient-to-r from-orange-500 via-red-500 to-yellow-500 rounded-full shadow-lg"></div>
                </div>

                <!-- Menu Items -->
                <div class="space-y-4 md:space-y-6 relative z-10">
                    {#each items as item}
                        <div class="menu-item bg-gray-800/50 backdrop-blur-sm p-4 md:p-6 rounded-2xl border border-orange-900/30 hover:border-orange-500/50 transition-all duration-300 group cursor-pointer" onclick={() => addToCart(item)}>
                            <div class="flex justify-between items-start">
                                <div class="flex-1">
                                    <h3 class="font-bold text-white text-lg md:text-xl mb-2 group-hover:text-orange-300 transition-colors duration-300">
                                        {item.name}
                                    </h3>
                                    {#if item.description}
                                        <p class="text-gray-300 text-sm leading-relaxed font-medium">
                                            {item.description}
                                        </p>
                                    {/if}
                                </div>
                                <div class="price-tag px-3 md:px-4 py-1 md:py-2 rounded-xl text-base md:text-lg font-black ml-4 md:ml-6 shadow-lg">
                                    ${item.price.toFixed(2)}
                                </div>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>
        {/each}
    </div>
</main>

<!-- Sparkle Effects -->
{#each sparkles as sparkle (sparkle.id)}
    <div
            class="sparkle fixed pointer-events-none z-50 text-3xl"
            style="left: {sparkle.x}px; top: {sparkle.y}px;"
    >
        🔥
    </div>
{/each}

<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');

    :global(body) {
        font-family: 'Inter', sans-serif;
        background-color: #000000;
        margin: 0;
        padding: 0;
    }

    .hero-gradient {
        background: linear-gradient(135deg, #1a1a1a 0%, #2d1b1b 50%, #1f1611 100%);
    }

    .nav-item.active {
        background: linear-gradient(135deg, #ea580c 0%, #dc2626 100%);
        box-shadow: 0 8px 25px rgba(234, 88, 12, 0.4);
        transform: translateX(8px);
    }

    .nav-item.active::before {
        content: '';
        position: absolute;
        left: -8px;
        top: 50%;
        transform: translateY(-50%);
        width: 4px;
        height: 80%;
        background: linear-gradient(to bottom, #f97316, #dc2626);
        border-radius: 2px;
        box-shadow: 0 0 10px rgba(249, 115, 22, 0.6);
    }

    .nav-item-mobile.active {
        background: linear-gradient(135deg, #ea580c 0%, #dc2626 100%);
        box-shadow: 0 4px 15px rgba(234, 88, 12, 0.4);
        transform: translateY(-4px);
    }

    .menu-section {
        transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        transform-origin: center;
    }

    .menu-section:hover,
    .menu-section.hovered {
        transform: translateY(-8px) scale(1.02);
        box-shadow:
                0 25px 50px rgba(234, 88, 12, 0.2),
                0 10px 25px rgba(220, 38, 38, 0.1),
                inset 0 1px 0 rgba(249, 115, 22, 0.2);
    }

    .menu-item {
        transition: all 0.3s ease-in-out;
    }

    .menu-item:hover {
        transform: translateX(12px) translateY(-4px);
        background: linear-gradient(135deg, rgba(55, 65, 81, 0.8) 0%, rgba(31, 41, 55, 0.9) 100%);
        box-shadow:
                0 15px 35px rgba(0, 0, 0, 0.3),
                0 5px 15px rgba(234, 88, 12, 0.2);
    }

    @media (max-width: 1024px) {
        .menu-item:hover {
            transform: translateY(-2px);
        }
    }

    .section-indicator {
        opacity: 0;
        transform: translateY(15px) translateX(15px);
        transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
        pointer-events: none;
    }

    .section-indicator.visible {
        opacity: 1;
        transform: translateY(0) translateX(0);
    }

    .price-tag {
        background: linear-gradient(135deg, #ea580c 0%, #dc2626 100%);
        color: white;
        transition: all 0.3s ease-in-out;
        border: 1px solid rgba(249, 115, 22, 0.3);
    }

    .menu-item:hover .price-tag {
        background: linear-gradient(135deg, #f97316 0%, #ef4444 100%);
        transform: scale(1.1) rotate(-2deg);
        box-shadow: 0 8px 25px rgba(234, 88, 12, 0.4);
    }

    .float-animation {
        animation: float 6s ease-in-out infinite;
    }

    .delay-2 {
        animation-delay: -2s;
    }

    .delay-4 {
        animation-delay: -4s;
    }

    @keyframes float {
        0%, 100% { transform: translateY(0px) rotate(0deg); }
        50% { transform: translateY(-15px) rotate(5deg); }
    }

    @keyframes slideInFromRight {
        0% {
            opacity: 0;
            transform: translateX(100px) scale(0.9);
        }
        100% {
            opacity: 1;
            transform: translateX(0) scale(1);
        }
    }

    .sparkle {
        animation: sparkle-float 1.5s ease-out forwards;
    }

    @keyframes sparkle-float {
        0% {
            opacity: 1;
            transform: translateY(0) scale(1) rotate(0deg);
        }
        100% {
            opacity: 0;
            transform: translateY(-30px) scale(0.3) rotate(180deg);
        }
    }

    /* Scroll animations */
    @keyframes pulse-glow {
        0%, 100% { box-shadow: 0 0 20px rgba(234, 88, 12, 0.3); }
        50% { box-shadow: 0 0 40px rgba(234, 88, 12, 0.6); }
    }

    .nav-item.active {
        animation: pulse-glow 2s ease-in-out infinite;
    }
</style>