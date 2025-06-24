<script>

    // Menu data using Svelte 5 runes
    import {onMount} from "svelte";

    let menuData = $state(
        {
            meals: [
                {
                    name: "Smash Burger With Fries",
                    description: "Grilled Onions, American Cheese, Muzzy's Sauce",
                    price: 10.99,
                    image: "/img_6_6.jpg"
                },
                {
                    name: "Bacon Smash Burger With Fries",
                    description: "Grilled Onions, American Cheese, Pickles, Muzzy's Sauce",
                    price: 11.99,
                    image: "/img_7_7.jpg"
                },
                {
                    name: "Chicken Slider With Fries",
                    description: "Kale Slaw, Pickles, American Cheese, Muzzy's Sauce",
                    price: 9.99,
                    image: "/img_8_8.jpg"
                },
                {
                    name: "2x Tenders With Fries",
                    description: "Slice of Bread, Pickles, Side of Muzzy's Sauce",
                    price: 12.99,
                    image: "/img_9_9.jpg"
                },
                {
                    name: "2x Chicken Slider With Fries",
                    description: "Kale Slaw, Pickles, American Cheese, Muzzy's Sauce",
                    price: 14.99,
                    image: "/img_10_10.jpg"
                },
                {
                    name: "1x Chicken Slider & 1x Tender With Fries",
                    description: "Kale Slaw, Pickles, American Cheese, Slice of Bread, Pickles, Side of Sauce",
                    price: 13.99,
                    image: "/img_11_11.jpg"
                },
                {
                    name: "Loaded Fries",
                    description: "Crispy fries loaded with cheese, bacon, and sauce",
                    price: 13.99,
                    image: "/img_12_12.jpg"
                }
            ],
            sides: [
                {
                    name: "Smash Burger",
                    price: 6.99,
                    image: "/img_13_13.jpg"
                },
                {
                    name: "Chicken Slider",
                    price: 6.99,
                    image: "/img_14_14.jpg"
                },
                {
                    name: "Fries",
                    price: 4.5,
                    image: "/img_15_15.jpg"
                },
                {
                    name: "Cheese Fries",
                    price: 5.5,
                    image: "/img_16_16.jpg"
                },
                {
                    name: "Mac Cheese",
                    price: 4.99,
                    image: "/img_17_17.jpg"
                },
                {
                    name: "Kale Slaw",
                    price: 4.99,
                    image: "/img_18_18.jpg"
                },
                {
                    name: "Tender",
                    price: 4.99,
                    image: "/img_19_19.jpg"
                },
                {
                    name: "Cup of Pickles",
                    price: 2.5,
                    image: "/img_20_20.jpg"
                },
                {
                    name: "Large Muzzy's Sauce",
                    price: 3.5,
                    image: "/img_21_21.jpg"
                },
                {
                    name: "Side of Muzzy's Sauce",
                    price: 0.38,
                    image: "/img_22_22.jpg"
                },
                {
                    name: "Side of Cheese Sauce",
                    price: 0.59,
                    image: "/img_23_23.jpg"
                }
            ],
            milkshakes: [
                {
                    name: "Milkshake",
                    price: 5.99,
                    image: "/img_24_24.jpg"
                },
                {
                    name: "M&Ms Milkshake",
                    price: 5.99,
                    image: "/img_25_25.jpg"
                },
                {
                    name: "Oreo Milkshake",
                    price: 5.99,
                    image: "/img_26_26.jpg"
                },
                {
                    name: "Strawberry Milkshake",
                    price: 5.99,
                    image: "/img_27_27.jpg"
                },
                {
                    name: "Chocolate Milkshake",
                    price: 5.99,
                    image: "/img_28_28.jpg"
                },
                {
                    name: "Vanilla Milkshake",
                    price: 5.99,
                    image: "/img_29_29.jpg"
                }
            ],
            desserts_ice_cream: [
                {
                    name: "Chocolate Ice Cream",
                    price: 5.99,
                    image: "/img_31_31.jpg"
                },
                {
                    name: "Vanilla Ice Cream",
                    price: 5.99,
                    image: "/img_32_32.jpg"
                },
                {
                    name: "Cookies & Cream Ice Cream",
                    price: 5.99,
                    image: "/img_33_33.jpg"
                },
                {
                    name: "Strawberry Ice Cream",
                    price: 5.99,
                    image: "/img_34_34.jpg"
                },
                {
                    name: "Banana Pudding",
                    price: 5.99,
                    image: "/img_35_35.jpg"
                }
            ],
            fountain_drinks: [
                {
                    name: "Ice Tea",
                    price: 3.2,
                    image: "/img_36_36.jpg"
                },
                {
                    name: "Ice Tea",
                    price: 3.2,
                    image: "/img_37_37.jpg"
                },
                {
                    name: "Lemonade",
                    price: 3.2,
                    image: "/img_38_38.jpg"
                },
                {
                    name: "Pink Lemonade",
                    price: 3.2,
                    image: "/img_39_39.jpg"
                },
                {
                    name: "Half & Half Drink",
                    price: 3.4,
                    image: "/img_40_40.jpg"
                }
            ],
            bottle_drinks: [
                {
                    name: "Water",
                    price: 1.99,
                    image: "/img_41_41.jpg"
                },
                {
                    name: "Mexican Coke",
                    price: 3.5,
                    image: "/img_42_42.jpg"
                },
                {
                    name: "Mexican Sprite",
                    price: 3.5,
                    image: "/img_43_43.jpg"
                },
                {
                    name: "Mexican Fanta",
                    price: 3.5,
                    image: "/img_44_44.jpg"
                },
                {
                    name: "20 oz. Coke",
                    price: 2.7,
                    image: "/img_45_45.jpg"
                },
                {
                    name: "20 oz. Diet Coke",
                    price: 2.7,
                    image: "/img_46_46.jpg"
                },
                {
                    name: "20 oz. Sprite",
                    price: 2.7,
                    image: "/img_47_47.jpg"
                },
                {
                    name: "20 oz. Dr Pepper",
                    price: 2.7,
                    image: "/img_48_48.jpg"
                },
                {
                    name: "20 oz. Diet Dr. Pepper",
                    price: 2.7,
                    image: "/img_49_49.jpg"
                }
            ]
        }
    );

    // Section configuration
    const sectionConfig = $state({
        meals: { title: 'Entrées', emoji: '🔥' },
        sides: { title: 'Sides', emoji: '🌶️' },
        fountain_drinks: { title: 'Fountain Drinks', emoji: '🥤' },
        milkshakes: { title: 'Milkshakes', emoji: '🥛' },
        bottle_drinks: { title: 'Bottled Drinks', emoji: '🍾' },
        desserts_ice_cream: { title: 'Desserts', emoji: '🍨' }
    });

    let cart = $state([]);
    let isCartOverlayOpen = $state(false);
    let activeSection = $state('Entrées');
    let checkedItems = $state(new Set());

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
            updateQuantity(existingIndex, 1);
        } else {
            cart = [...cart, { ...item, quantity: 1 }];
        }

        // Show checkmark overlay
        showCheckmark(item.name);
    }

    function showCheckmark(itemName) {
        // Add item to checked set
        checkedItems = new Set([...checkedItems, itemName]);

        // Remove checkmark after 1 second
        setTimeout(() => {
            checkedItems = new Set([...checkedItems].filter(name => name !== itemName));
        }, 1000);
    }

    function updateQuantity(index, change) {
        cart = cart.map((item, i) => {
            if (i === index) {
                const newQuantity = (item.quantity || 1) + change;
                if (newQuantity <= 0) {
                    return null;
                }
                return { ...item, quantity: newQuantity };
            }
            return item;
        }).filter(item => item !== null);
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
                    }
                });
            }, {
                rootMargin: '-20% 0px -50% 0px',
                threshold: 0.1
            });

            observer.observe(target);
            observers.set(targetId, observer);
        });

        return () => {
            observers.forEach(observer => observer.disconnect());
        };
    });
</script>

<style>
    .checkmark-overlay {
        animation: checkmarkPulse 1s ease-out;
    }

    @keyframes checkmarkPulse {
        0% {
            opacity: 0;
            transform: scale(0.5);
        }
        50% {
            opacity: 1;
            transform: scale(1.1);
        }
        100% {
            opacity: 1;
            transform: scale(1);
        }
    }

    .checkmark-circle {
        animation: checkmarkCircle 0.6s ease-out;
    }

    @keyframes checkmarkCircle {
        0% {
            border-width: 0;
            transform: scale(0);
        }
        50% {
            border-width: 3px;
            transform: scale(1.1);
        }
        100% {
            border-width: 3px;
            transform: scale(1);
        }
    }

    .checkmark-check {
        animation: checkmarkCheck 0.3s 0.3s ease-out both;
    }

    @keyframes checkmarkCheck {
        0% {
            stroke-dashoffset: 16;
        }
        100% {
            stroke-dashoffset: 0;
        }
    }
</style>

<!-- Header -->
<header class="bg-white py-12 border-b border-gray-100">
    <div class="container mx-auto px-6 text-center">
        <h1 class="text-4xl md:text-6xl font-black mb-4 text-orange-600">
            Muzzy's Shack
        </h1>
        <p class="text-lg text-gray-600 max-w-xl mx-auto">
            🔥 Blazing Hot Chicken & More 🔥
        </p>
    </div>
</header>

<!-- Desktop Cart (Hidden on Mobile) -->
<div class="hidden lg:block fixed top-0 right-0 z-50 w-1/4">
    <div class="bg-white rounded-xl shadow-xl border border-gray-200 h-screen flex flex-col justify-between">
        <!-- Cart Header -->
        <div class="bg-orange-600 text-white p-4 rounded-t-xl">
            <div class="flex items-center justify-between">
                <h3 class="font-bold text-lg">🛒 Your Order</h3>
                <span class="bg-white/20 px-2 py-1 rounded-full text-sm font-medium">
                        {cart.length} items
                    </span>
            </div>
        </div>
        {#if cart.length <= 0}
            <div class="flex items-center justify-center w-full h-full text-center">
                <h2 class="text-gray-300 text-4xl">What are you waiting for?<br/>Add some delicious meals!</h2>
            </div>
        {/if}
        <!-- Cart Items -->
        <div class="flex justify-between flex-col h-full">
            <div class="max-h-full overflow-y-auto p-4 space-y-3 ">
                {#each cart as item, index}
                    <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                        <img src={item.image} alt={item.title} class="rounded-sm w-32 px-4" />
                        <div class="flex-1">
                            <h4 class="font-medium text-black text-sm">{item.name}</h4>
                            <div class="flex items-center gap-2 mt-2">
                                <button
                                        class="w-6 h-6 rounded-full bg-orange-100 text-orange-600 flex items-center justify-center text-sm font-bold hover:bg-orange-200"
                                        onclick={() => updateQuantity(index, -1)}
                                >
                                    −
                                </button>
                                <span class="text-black font-medium text-sm w-8 text-center">
                                    {item.quantity || 1}
                                </span>
                                <button
                                        class="w-6 h-6 rounded-full bg-orange-100 text-orange-600 flex items-center justify-center text-sm font-bold hover:bg-orange-200"
                                        onclick={() => updateQuantity(index, 1)}
                                >
                                    +
                                </button>
                                <button
                                        class="ml-2 text-red-500 hover:text-red-700 text-sm"
                                        onclick={() => removeFromCart(index)}
                                >
                                    🗑️
                                </button>
                            </div>
                        </div>
                        <div class="text-right ml-4">
                            <div class="bg-orange-600 text-white px-2 py-1 rounded font-bold text-sm">
                                ${((item.price || 0) * (item.quantity || 1)).toFixed(2)}
                            </div>
                        </div>
                    </div>
                {/each}
            </div>

            <!-- Cart Summary -->
            <div class="p-4 border-t border-gray-200">
                <div class="space-y-2 mb-4">
                    <div class="flex justify-between text-black">
                        <span>Subtotal:</span>
                        <span class="font-medium">${cartTotal.toFixed(2)}</span>
                    </div>
                    <div class="flex justify-between text-gray-600 text-sm">
                        <span>Tax (8.25%):</span>
                        <span>${(cartTotal * 0.0825).toFixed(2)}</span>
                    </div>
                    <div class="border-t pt-2 flex justify-between text-black font-bold">
                        <span>Total:</span>
                        <span class="text-orange-600">${(cartTotal * 1.0825).toFixed(2)}</span>
                    </div>
                </div>
                <div class="space-y-2">
                    <button class="w-full bg-orange-600 hover:bg-orange-700 text-white font-bold py-3 rounded-lg">
                        🔥 Checkout Now
                    </button>
                    <button
                            class="w-full bg-gray-200 hover:bg-gray-300 text-gray-700 font-medium py-2 rounded-lg"
                            onclick={() => clearCart()}
                    >
                        Clear Cart
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>


<!-- Mobile Cart Overlay -->
{#if isCartOverlayOpen}
    <div class="lg:hidden fixed inset-0 z-50 bg-black/50" onclick={closeCartOverlay}>
        <div class="h-full flex flex-col bg-white" onclick={(e) => e.stopPropagation()}>
            <!-- Cart Header -->
            <div class="bg-orange-600 text-white p-6 flex items-center justify-between">
                <div>
                    <h2 class="font-black text-xl">🛒 Your Order</h2>
                    <p class="text-orange-100 text-sm">{cartItemCount} items • ${cartTotal.toFixed(2)}</p>
                </div>
                <button class="text-white p-2" onclick={closeCartOverlay}>✕</button>
            </div>

            <!-- Cart Items -->
            <div class="flex-1 overflow-y-auto p-4">
                {#if cart.length === 0}
                    <div class="text-center text-gray-500 mt-20">
                        <div class="text-4xl mb-4">🛒</div>
                        <p class="text-lg">Your cart is empty</p>
                    </div>
                {:else}
                    <div class="space-y-4">
                        {#each cart as item, index}
                            <div class="bg-gray-50 rounded-lg p-4">
                                <div class="flex justify-between items-start mb-4">
                                    <div class="flex-1">
                                        <h3 class="font-bold text-black">{item.name}</h3>
                                        {#if item.description}
                                            <p class="text-gray-600 text-sm mt-1">{item.description}</p>
                                        {/if}
                                    </div>
                                    <button
                                            class="text-red-500 hover:text-red-700 p-2"
                                            onclick={() => removeFromCart(index)}
                                    >
                                        🗑️
                                    </button>
                                </div>
                                <div class="flex justify-between items-center">
                                    <div class="flex items-center gap-3">
                                        <button
                                                class="w-8 h-8 rounded-full bg-orange-100 text-orange-600 flex items-center justify-center font-bold hover:bg-orange-200"
                                                onclick={() => updateQuantity(index, -1)}
                                        >
                                            −
                                        </button>
                                        <span class="text-black font-bold w-8 text-center">
                                            {item.quantity || 1}
                                        </span>
                                        <button
                                                class="w-8 h-8 rounded-full bg-orange-100 text-orange-600 flex items-center justify-center font-bold hover:bg-orange-200"
                                                onclick={() => updateQuantity(index, 1)}
                                        >
                                            +
                                        </button>
                                    </div>
                                    <div class="bg-orange-600 text-white px-3 py-2 rounded font-bold">
                                        ${((item.price || 0) * (item.quantity || 1)).toFixed(2)}
                                    </div>
                                </div>
                            </div>
                        {/each}
                    </div>
                {/if}
            </div>

            <!-- Cart Footer -->
            {#if cart.length > 0}
                <div class="p-6 border-t border-gray-200">
                    <div class="space-y-2 mb-6">
                        <div class="flex justify-between text-black">
                            <span>Subtotal:</span>
                            <span>${cartTotal.toFixed(2)}</span>
                        </div>
                        <div class="flex justify-between text-gray-600 text-sm">
                            <span>Tax (8.25%):</span>
                            <span>${(cartTotal * 0.0825).toFixed(2)}</span>
                        </div>
                        <div class="border-t pt-3 flex justify-between text-black font-bold text-lg">
                            <span>Total:</span>
                            <span class="text-orange-600">${(cartTotal * 1.0825).toFixed(2)}</span>
                        </div>
                    </div>
                    <div class="space-y-3">
                        <button class="w-full bg-orange-600 hover:bg-orange-700 text-white font-bold py-4 rounded-lg text-lg">
                            🔥 Checkout Now
                        </button>
                        <button
                                class="w-full bg-gray-200 hover:bg-gray-300 text-gray-700 font-medium py-3 rounded-lg"
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

<!-- Main Content -->
<main class="bg-white min-h-screen">
    <!-- Desktop Sidebar Navigation -->
    <nav class="hidden lg:block fixed left-6 top-1/2 transform -translate-y-1/2 z-40">
        <div class="bg-white rounded-xl shadow-lg border border-gray-200 p-4">
            <ul class="space-y-2">
                {#each menuSections as [sectionKey, items]}
                    {@const config = sectionConfig[sectionKey]}
                    <li>
                        <button
                                class="w-full text-left p-3 rounded-lg transition-all duration-200 font-medium text-sm {activeSection === config.title ? 'bg-orange-600 text-white' : 'text-gray-700 hover:bg-gray-50'}"
                                onclick={() => scrollToSection(config.title)}
                        >
                            <span class="flex items-center gap-3">
                                <span class="text-base">{config.emoji}</span>
                                <span>{config.title}</span>
                            </span>
                        </button>
                    </li>
                {/each}
            </ul>
        </div>
    </nav>

    <!-- Mobile Bottom Navigation -->
    <nav class="lg:hidden fixed bottom-0 left-0 right-0 z-40 bg-white border-t border-gray-200 px-2 py-3">
        <div class="flex justify-between items-center max-w-lg mx-auto">
            {#each menuSections as [sectionKey, items]}
                {@const config = sectionConfig[sectionKey]}
                <button
                        class="flex flex-col items-center p-2 rounded-lg transition-all duration-200 {activeSection === config.title ? 'bg-orange-100 text-orange-600' : 'text-gray-600'}"
                        onclick={() => scrollToSection(config.title)}
                >
                    <span class="text-lg mb-1">{config.emoji}</span>
                    <span class="text-xs font-medium text-center leading-tight">
                        {config.title.split(' ')[0]}
                    </span>
                </button>
            {/each}

            <button
                    class="flex flex-col items-center p-2 rounded-lg transition-all duration-200 {cart.length > 0 ? 'bg-orange-100 text-orange-600' : 'text-gray-600'}"
                    onclick={toggleCartOverlay}
            >
                <div class="relative">
                    <span class="text-lg mb-1">🛒</span>
                    {#if cartItemCount > 0}
                        <span class="absolute -top-1 -right-1 bg-orange-600 text-white text-xs font-bold rounded-full w-4 h-4 flex items-center justify-center">
                            {cartItemCount > 9 ? '9+' : cartItemCount}
                        </span>
                    {/if}
                </div>
                <span class="text-xs font-medium">Cart</span>
            </button>
        </div>
    </nav>

    <!-- Menu Content -->
    <div class="lg:ml-64 container mx-auto px-4 py-8 pb-20 lg:pb-8">
        {#each menuSections as [sectionKey, items]}
            {@const config = sectionConfig[sectionKey]}
            <div class="mb-12" id={config.title+'-section'}>
                <!-- Section Header -->
                <div class="mb-8">
                    <h2 class="text-3xl font-black text-black mb-2 flex items-center gap-3">
                        <span class="text-2xl">{config.emoji}</span>
                        {config.title}
                    </h2>
                    <div class="w-16 h-1 bg-orange-600 rounded-full"></div>
                </div>

                <!-- Menu Items Grid -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    {#each items as item}
                        <div
                                class="bg-white rounded-xl border border-gray-200 overflow-hidden hover:shadow-lg transition-all duration-200 cursor-pointer group relative"
                                onclick={() => addToCart(item)}
                                aria-hidden="true"
                        >
                            <!-- Item Image -->
                            <div class="w-full h-48 bg-gray-100 overflow-hidden">
                                <img
                                        src={item.image}
                                        alt={item.name}
                                        class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-200"
                                        loading="lazy"
                                />
                            </div>

                            <!-- Item Content -->
                            <div class="p-4">
                                <div class="flex justify-between items-start mb-2">
                                    <h3 class="font-bold text-black text-lg leading-tight flex-1">
                                        {item.name}
                                    </h3>
                                    <div class="bg-orange-600 text-white px-3 py-1 rounded-full font-bold text-sm ml-3">
                                        ${item.price.toFixed(2)}
                                    </div>
                                </div>
                                {#if item.description}
                                    <p class="text-gray-600 text-sm leading-relaxed">
                                        {item.description}
                                    </p>
                                {/if}
                            </div>

                            <!-- Checkmark Overlay -->
                            {#if checkedItems.has(item.name)}
                                <div class="absolute inset-0 bg-orange-600/90 flex items-center justify-center rounded-xl checkmark-overlay z-10">
                                    <div class="text-center">
                                        <div class="relative mb-2">
                                            <div class="w-16 h-16 rounded-full border-white checkmark-circle mx-auto flex items-center justify-center">
                                                <svg class="w-8 h-8 text-white checkmark-check" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" stroke-dasharray="30" stroke-dashoffset="20">
                                                    <polyline points="20,6 9,17 4,12"></polyline>
                                                </svg>
                                            </div>
                                        </div>
                                        <p class="text-white font-bold text-lg">Added to Cart!</p>
                                    </div>
                                </div>
                            {/if}
                        </div>
                    {/each}
                </div>
            </div>
        {/each}
    </div>
</main>