import React, { useState, useEffect, useMemo } from 'react';
import { 
  ShoppingBag, 
  Search, 
  Menu, 
  X, 
  Instagram, 
  Twitter, 
  Facebook, 
  ChevronRight, 
  Palette, 
  Info,
  CheckCircle,
  Heart
} from 'lucide-react';

// Mock Data for Art Pieces
const ART_DATA = [
  {
    id: 1,
    title: "Eternal Gaze",
    category: "Sketch",
    price: 120,
    image: "https://images.unsplash.com/photo-1579783902614-a3fb3927b6a5?auto=format&fit=crop&q=80&w=800",
    description: "A detailed charcoal sketch focusing on the emotional depth of the human eye.",
    size: "12x16 inches",
    medium: "Charcoal on Acid-free Paper"
  },
  {
    id: 2,
    title: "Vibrant Horizon",
    category: "Painting",
    price: 450,
    image: "https://images.unsplash.com/photo-1541963463532-d68292c34b19?auto=format&fit=crop&q=80&w=800",
    description: "Oil on canvas exploring the intersection of light and shadow during a coastal sunset.",
    size: "24x36 inches",
    medium: "Oil Painting"
  },
  {
    id: 3,
    title: "Abstract Rhythm",
    category: "Abstract",
    price: 320,
    image: "https://images.unsplash.com/photo-1541701494587-cb58502866ab?auto=format&fit=crop&q=80&w=800",
    description: "A rhythmic flow of acrylic colors representing the chaos of city life.",
    size: "20x20 inches",
    medium: "Acrylic on Wood"
  },
  {
    id: 4,
    title: "Minimalist Form",
    category: "Sketch",
    price: 85,
    image: "https://images.unsplash.com/photo-1513364776144-60967b0f800f?auto=format&fit=crop&q=80&w=800",
    description: "Single line ink drawing exploring the elegance of the human silhouette.",
    size: "8x10 inches",
    medium: "India Ink on Bristol Board"
  },
  {
    id: 5,
    title: "Forest Whisper",
    category: "Painting",
    price: 580,
    image: "https://images.unsplash.com/photo-1578301978693-85fa9c0320b9?auto=format&fit=crop&q=80&w=800",
    description: "An atmospheric landscape capturing the morning mist in a pine forest.",
    size: "30x40 inches",
    medium: "Oil on Canvas"
  },
  {
    id: 6,
    title: "Urban Geometry",
    category: "Abstract",
    price: 210,
    image: "https://images.unsplash.com/photo-1549490349-8643362247b5?auto=format&fit=crop&q=80&w=800",
    description: "Hard-edge abstraction focused on architectural perspectives.",
    size: "18x24 inches",
    medium: "Mixed Media"
  }
];

const App = () => {
  const [view, setView] = useState('home'); // home, gallery, details, cart
  const [filter, setFilter] = useState('All');
  const [selectedArt, setSelectedArt] = useState(null);
  const [cart, setCart] = useState([]);
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [searchQuery, setSearchQuery] = useState('');
  const [notification, setNotification] = useState(null);

  // Filter Logic
  const filteredArt = useMemo(() => {
    return ART_DATA.filter(item => {
      const matchesCategory = filter === 'All' || item.category === filter;
      const matchesSearch = item.title.toLowerCase().includes(searchQuery.toLowerCase());
      return matchesCategory && matchesSearch;
    });
  }, [filter, searchQuery]);

  const addToCart = (art) => {
    if (!cart.find(item => item.id === art.id)) {
      setCart([...cart, art]);
      showNotification(`Added "${art.title}" to cart!`);
    } else {
      showNotification("Item already in cart.");
    }
  };

  const removeFromCart = (id) => {
    setCart(cart.filter(item => item.id !== id));
  };

  const showNotification = (msg) => {
    setNotification(msg);
    setTimeout(() => setNotification(null), 3000);
  };

  const totalPrice = cart.reduce((acc, curr) => acc + curr.price, 0);

  // Navigation Helper
  const navigate = (newView, art = null) => {
    setView(newView);
    if (art) setSelectedArt(art);
    window.scrollTo(0, 0);
    setIsMenuOpen(false);
  };

  return (
    <div className="min-h-screen bg-stone-50 text-stone-900 font-sans selection:bg-amber-200">
      {/* Notification Toast */}
      {notification && (
        <div className="fixed top-20 right-4 z-50 bg-stone-900 text-white px-6 py-3 rounded-full shadow-2xl flex items-center gap-2 animate-bounce">
          <CheckCircle className="w-5 h-5 text-amber-400" />
          {notification}
        </div>
      )}

      {/* Navigation */}
      <nav className="fixed top-0 w-full bg-white/80 backdrop-blur-md z-40 border-b border-stone-200">
        <div className="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
          <div 
            className="flex items-center gap-2 cursor-pointer group"
            onClick={() => navigate('home')}
          >
            <Palette className="w-8 h-8 text-amber-700 group-hover:rotate-12 transition-transform" />
            <h1 className="text-xl font-serif font-bold tracking-tight">ARTISTIC<span className="text-amber-700">SOUL</span></h1>
          </div>

          {/* Desktop Menu */}
          <div className="hidden md:flex items-center gap-8 font-medium text-sm uppercase tracking-widest">
            <button onClick={() => navigate('home')} className="hover:text-amber-700 transition-colors">Home</button>
            <button onClick={() => navigate('gallery')} className="hover:text-amber-700 transition-colors">Gallery</button>
            <button className="hover:text-amber-700 transition-colors">Commissions</button>
            <button className="hover:text-amber-700 transition-colors">About</button>
          </div>

          <div className="flex items-center gap-4">
            <button 
              onClick={() => navigate('cart')}
              className="relative p-2 hover:bg-stone-100 rounded-full transition-colors"
            >
              <ShoppingBag className="w-6 h-6" />
              {cart.length > 0 && (
                <span className="absolute top-0 right-0 bg-amber-600 text-white text-[10px] w-5 h-5 flex items-center justify-center rounded-full font-bold">
                  {cart.length}
                </span>
              )}
            </button>
            <button 
              className="md:hidden"
              onClick={() => setIsMenuOpen(!isMenuOpen)}
            >
              {isMenuOpen ? <X /> : <Menu />}
            </button>
          </div>
        </div>
      </nav>

      {/* Mobile Menu Overlay */}
      {isMenuOpen && (
        <div className="fixed inset-0 bg-white z-50 p-8 flex flex-col gap-6 text-2xl font-serif">
          <button className="self-end" onClick={() => setIsMenuOpen(false)}><X size={32}/></button>
          <button onClick={() => navigate('home')}>Home</button>
          <button onClick={() => navigate('gallery')}>The Collection</button>
          <button onClick={() => setIsMenuOpen(false)}>Commissions</button>
          <button onClick={() => setIsMenuOpen(false)}>Biography</button>
          <div className="mt-auto flex gap-6">
            <Instagram className="text-stone-400" />
            <Twitter className="text-stone-400" />
            <Facebook className="text-stone-400" />
          </div>
        </div>
      )}

      {/* Main Content Area */}
      <main className="pt-16">
        {view === 'home' && (
          <div>
            {/* Hero Section */}
            <section className="relative h-[80vh] flex items-center overflow-hidden">
              <div className="absolute inset-0 z-0">
                <img 
                  src="https://images.unsplash.com/photo-1460661419201-fd4cecdf8a8b?auto=format&fit=crop&q=80&w=1600" 
                  className="w-full h-full object-cover opacity-20"
                  alt="Background"
                />
              </div>
              <div className="relative z-10 max-w-7xl mx-auto px-4 grid md:grid-cols-2 gap-12 items-center">
                <div className="space-y-6">
                  <span className="inline-block px-4 py-1 bg-amber-100 text-amber-800 rounded-full text-xs font-bold uppercase tracking-widest">
                    Spring Collection 2024
                  </span>
                  <h2 className="text-5xl md:text-7xl font-serif leading-tight">
                    Where Every <span className="italic text-amber-700">Stroke</span> Tells a Story.
                  </h2>
                  <p className="text-lg text-stone-600 max-w-md">
                    Original charcoal sketches, oil paintings, and contemporary abstracts delivered from the studio to your sanctuary.
                  </p>
                  <div className="flex gap-4">
                    <button 
                      onClick={() => navigate('gallery')}
                      className="bg-stone-900 text-white px-8 py-4 rounded-full font-medium hover:bg-stone-800 transition-all flex items-center gap-2"
                    >
                      Explore Gallery <ChevronRight size={18} />
                    </button>
                    <button className="border border-stone-300 px-8 py-4 rounded-full font-medium hover:bg-white transition-all">
                      Request Commission
                    </button>
                  </div>
                </div>
                <div className="hidden md:block relative">
                  <div className="absolute -inset-4 border border-amber-200 rounded-2xl rotate-3"></div>
                  <img 
                    src="https://images.unsplash.com/photo-1579783902614-a3fb3927b6a5?auto=format&fit=crop&q=80&w=800" 
                    className="relative rounded-2xl shadow-2xl transform -rotate-2 hover:rotate-0 transition-transform duration-500"
                    alt="Featured Art"
                  />
                </div>
              </div>
            </section>

            {/* Featured Categories */}
            <section className="py-24 max-w-7xl mx-auto px-4">
              <div className="flex justify-between items-end mb-12">
                <div>
                  <h3 className="text-3xl font-serif">Curated Collections</h3>
                  <p className="text-stone-500 mt-2">Hand-picked selections from the artist.</p>
                </div>
                <button onClick={() => navigate('gallery')} className="text-amber-700 font-bold border-b-2 border-amber-700 pb-1">View All Works</button>
              </div>
              <div className="grid md:grid-cols-3 gap-8">
                {['Sketch', 'Painting', 'Abstract'].map((cat, idx) => {
                  const item = ART_DATA.find(a => a.category === cat);
                  return (
                    <div 
                      key={cat} 
                      className="group cursor-pointer overflow-hidden rounded-xl relative h-96"
                      onClick={() => { setFilter(cat); navigate('gallery'); }}
                    >
                      <img 
                        src={item.image} 
                        className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-700" 
                        alt={cat}
                      />
                      <div className="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent p-8 flex flex-col justify-end">
                        <h4 className="text-white text-2xl font-serif">{cat}es</h4>
                        <p className="text-stone-300 text-sm mt-2 opacity-0 group-hover:opacity-100 transition-opacity">Explore {cat} Collection</p>
                      </div>
                    </div>
                  );
                })}
              </div>
            </section>
          </div>
        )}

        {view === 'gallery' && (
          <section className="max-w-7xl mx-auto px-4 py-12">
            <div className="flex flex-col md:flex-row md:items-center justify-between gap-6 mb-12">
              <h2 className="text-4xl font-serif">The Collection</h2>
              
              <div className="flex flex-wrap items-center gap-4">
                {/* Search */}
                <div className="relative">
                  <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-stone-400 w-4 h-4" />
                  <input 
                    type="text" 
                    placeholder="Search art..." 
                    className="pl-10 pr-4 py-2 bg-white border border-stone-200 rounded-full text-sm focus:outline-none focus:ring-2 focus:ring-amber-500/20"
                    value={searchQuery}
                    onChange={(e) => setSearchQuery(e.target.value)}
                  />
                </div>
                {/* Filters */}
                <div className="flex bg-stone-200/50 p-1 rounded-full text-sm">
                  {['All', 'Sketch', 'Painting', 'Abstract'].map(cat => (
                    <button
                      key={cat}
                      onClick={() => setFilter(cat)}
                      className={`px-4 py-1.5 rounded-full transition-all ${filter === cat ? 'bg-white shadow-sm text-stone-900 font-bold' : 'text-stone-500 hover:text-stone-700'}`}
                    >
                      {cat}
                    </button>
                  ))}
                </div>
              </div>
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
              {filteredArt.map((art) => (
                <div 
                  key={art.id} 
                  className="group"
                >
                  <div 
                    className="relative aspect-[3/4] overflow-hidden rounded-2xl bg-stone-200 cursor-pointer"
                    onClick={() => navigate('details', art)}
                  >
                    <img 
                      src={art.image} 
                      alt={art.title} 
                      className="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                    />
                    <div className="absolute top-4 right-4 flex flex-col gap-2">
                      <button className="p-2 bg-white/90 backdrop-blur rounded-full hover:bg-amber-50 transition-colors shadow-sm text-stone-600 hover:text-rose-500">
                        <Heart className="w-4 h-4" />
                      </button>
                    </div>
                  </div>
                  <div className="mt-4 flex justify-between items-start">
                    <div>
                      <p className="text-xs uppercase tracking-widest text-amber-700 font-bold mb-1">{art.category}</p>
                      <h4 className="text-lg font-serif font-medium">{art.title}</h4>
                      <p className="text-stone-500 text-sm mt-1">${art.price}</p>
                    </div>
                    <button 
                      onClick={() => addToCart(art)}
                      className="p-3 bg-stone-900 text-white rounded-full hover:bg-amber-700 transition-colors"
                    >
                      <ShoppingBag className="w-5 h-5" />
                    </button>
                  </div>
                </div>
              ))}
            </div>
            {filteredArt.length === 0 && (
              <div className="py-20 text-center">
                <p className="text-stone-400 text-lg">No pieces found matching your criteria.</p>
                <button onClick={() => {setFilter('All'); setSearchQuery('');}} className="mt-4 text-amber-700 font-bold underline">Reset Filters</button>
              </div>
            )}
          </section>
        )}

        {view === 'details' && selectedArt && (
          <section className="max-w-7xl mx-auto px-4 py-12">
            <button 
              onClick={() => navigate('gallery')}
              className="mb-8 flex items-center gap-2 text-stone-500 hover:text-stone-900 transition-colors"
            >
              <ChevronRight className="rotate-180 w-4 h-4" /> Back to Gallery
            </button>
            <div className="grid md:grid-cols-2 gap-12">
              <div className="space-y-4">
                <div className="rounded-3xl overflow-hidden bg-white shadow-xl">
                  <img 
                    src={selectedArt.image} 
                    alt={selectedArt.title} 
                    className="w-full h-auto"
                  />
                </div>
                <div className="grid grid-cols-3 gap-4">
                  {[...Array(3)].map((_, i) => (
                    <div key={i} className="aspect-square bg-stone-200 rounded-xl overflow-hidden cursor-pointer hover:opacity-80">
                      <img src={selectedArt.image} className="w-full h-full object-cover grayscale opacity-50" alt="detail" />
                    </div>
                  ))}
                </div>
              </div>
              <div className="flex flex-col">
                <span className="text-amber-700 font-bold tracking-widest uppercase text-sm mb-2">{selectedArt.category}</span>
                <h2 className="text-4xl md:text-5xl font-serif mb-4">{selectedArt.title}</h2>
                <p className="text-3xl text-stone-400 mb-8">${selectedArt.price}</p>
                
                <div className="bg-white p-6 rounded-2xl border border-stone-200 space-y-4 mb-8">
                  <div className="flex items-center gap-3">
                    <Info className="text-amber-600 w-5 h-5" />
                    <span className="font-bold text-sm uppercase">Artwork Details</span>
                  </div>
                  <div className="grid grid-cols-2 gap-y-3 text-sm">
                    <span className="text-stone-400">Dimensions</span>
                    <span className="text-stone-800">{selectedArt.size}</span>
                    <span className="text-stone-400">Medium</span>
                    <span className="text-stone-800">{selectedArt.medium}</span>
                    <span className="text-stone-400">Authenticity</span>
                    <span className="text-stone-800">Signed Original</span>
                  </div>
                </div>

                <p className="text-stone-600 leading-relaxed mb-8">
                  {selectedArt.description}
                </p>

                <div className="mt-auto space-y-4">
                  <button 
                    onClick={() => addToCart(selectedArt)}
                    className="w-full bg-stone-900 text-white py-4 rounded-full text-lg font-bold hover:bg-stone-800 transition-all flex items-center justify-center gap-3"
                  >
                    Add to Collection <ShoppingBag size={20} />
                  </button>
                  <p className="text-center text-stone-400 text-xs flex items-center justify-center gap-2">
                    <CheckCircle size={14} className="text-emerald-500" /> Free insured international shipping
                  </p>
                </div>
              </div>
            </div>
          </section>
        )}

        {view === 'cart' && (
          <section className="max-w-3xl mx-auto px-4 py-12">
            <h2 className="text-4xl font-serif mb-8">Your Collection</h2>
            {cart.length === 0 ? (
              <div className="text-center py-20 bg-white rounded-3xl border border-dashed border-stone-300">
                <ShoppingBag className="w-16 h-16 text-stone-200 mx-auto mb-4" />
                <p className="text-stone-400 text-lg mb-6">Your cart is currently empty.</p>
                <button 
                  onClick={() => navigate('gallery')}
                  className="bg-stone-900 text-white px-8 py-3 rounded-full font-medium"
                >
                  Browse Gallery
                </button>
              </div>
            ) : (
              <div className="space-y-6">
                {cart.map(item => (
                  <div key={item.id} className="flex gap-6 bg-white p-4 rounded-2xl border border-stone-100 shadow-sm">
                    <img src={item.image} className="w-24 h-24 object-cover rounded-xl" alt={item.title} />
                    <div className="flex-1">
                      <h4 className="font-serif text-xl">{item.title}</h4>
                      <p className="text-stone-400 text-sm">{item.category} • {item.size}</p>
                      <p className="font-bold text-amber-700 mt-2">${item.price}</p>
                    </div>
                    <button 
                      onClick={() => removeFromCart(item.id)}
                      className="p-2 text-stone-300 hover:text-rose-500 transition-colors"
                    >
                      <X size={20} />
                    </button>
                  </div>
                ))}

                <div className="mt-12 bg-stone-900 text-white p-8 rounded-3xl">
                  <div className="flex justify-between items-center mb-6">
                    <span className="text-stone-400 uppercase tracking-widest text-sm font-bold">Subtotal</span>
                    <span className="text-3xl font-serif">${totalPrice}</span>
                  </div>
                  <button className="w-full bg-amber-600 hover:bg-amber-500 text-white py-4 rounded-full font-bold transition-all">
                    Proceed to Secure Checkout
                  </button>
                  <p className="text-center mt-4 text-xs text-stone-500">
                    Calculated at next step: Shipping, taxes, and duties.
                  </p>
                </div>
              </div>
            )}
          </section>
        )}
      </main>

      {/* Footer */}
      <footer className="bg-stone-100 mt-24 border-t border-stone-200">
        <div className="max-w-7xl mx-auto px-4 py-16 grid md:grid-cols-4 gap-12">
          <div className="col-span-2">
            <div className="flex items-center gap-2 mb-6">
              <Palette className="w-6 h-6 text-amber-700" />
              <h2 className="text-lg font-serif font-bold tracking-tight">ARTISTIC<span className="text-amber-700">SOUL</span></h2>
            </div>
            <p className="text-stone-500 max-w-sm mb-6">
              A private studio dedicated to the exploration of human emotion through visual media. All works are hand-created and authenticated by the artist.
            </p>
            <div className="flex gap-4">
              <Instagram className="w-5 h-5 text-stone-400 hover:text-amber-700 cursor-pointer" />
              <Twitter className="w-5 h-5 text-stone-400 hover:text-amber-700 cursor-pointer" />
              <Facebook className="w-5 h-5 text-stone-400 hover:text-amber-700 cursor-pointer" />
            </div>
          </div>
          <div>
            <h4 className="font-bold mb-6 uppercase text-xs tracking-widest">Navigation</h4>
            <ul className="space-y-3 text-sm text-stone-600">
              <li className="hover:text-amber-700 cursor-pointer" onClick={() => navigate('gallery')}>The Gallery</li>
              <li className="hover:text-amber-700 cursor-pointer">Commission a Piece</li>
              <li className="hover:text-amber-700 cursor-pointer">Artist Biography</li>
              <li className="hover:text-amber-700 cursor-pointer">Exhibitions</li>
            </ul>
          </div>
          <div>
            <h4 className="font-bold mb-6 uppercase text-xs tracking-widest">Support</h4>
            <ul className="space-y-3 text-sm text-stone-600">
              <li className="hover:text-amber-700 cursor-pointer">Shipping & Logistics</li>
              <li className="hover:text-amber-700 cursor-pointer">Returns Policy</li>
              <li className="hover:text-amber-700 cursor-pointer">Contact Studio</li>
              <li className="hover:text-amber-700 cursor-pointer">Privacy Policy</li>
            </ul>
          </div>
        </div>
        <div className="border-t border-stone-200 py-8 text-center text-xs text-stone-400">
          © 2024 Artistic Soul Studio. All rights reserved. Built for visual storytellers.
        </div>
      </footer>
    </div>
  );
};

export default App;
