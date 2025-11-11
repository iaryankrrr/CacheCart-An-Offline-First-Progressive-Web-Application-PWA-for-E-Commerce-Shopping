import React, { useState, useEffect } from 'react';
import { ShoppingBag, Heart, WifiOff } from 'lucide-react';

export default function CacheCartApp() {
  const [offline, setOffline] = useState(false);
  const [cart, setCart] = useState([]);

  const products = [
    { id: 1, name: 'Men Black Jacket', price: 1999, image: 'https://images.unsplash.com/photo-1521572163474-6864f9cf17ab' },
    { id: 2, name: 'Women Red Dress', price: 2499, image: 'https://images.unsplash.com/photo-1520975922370-1f4a01ce2c2a' },
    { id: 3, name: 'Casual Sneakers', price: 1499, image: 'https://images.unsplash.com/photo-1528701800489-20be3c1b3f7e' },
    { id: 4, name: 'Smart Watch', price: 2999, image: 'https://images.unsplash.com/photo-1511389026070-a14ae610a1be' },
  ];

  useEffect(() => {
    window.addEventListener('offline', () => setOffline(true));
    window.addEventListener('online', () => setOffline(false));

    // Register Service Worker for PWA functionality
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('/service-worker.js')
        .then(() => console.log('Service Worker Registered'))
        .catch(err => console.error('Service Worker registration failed:', err));
    }
  }, []);

  const addToCart = (product) => {
    setCart([...cart, product]);
  };

  return (
    <div className="min-h-screen bg-gray-50 font-sans">
      {/* Navbar */}
      <header className="flex justify-between items-center bg-white px-6 py-4 shadow-md sticky top-0 z-10">
        <h1 className="text-3xl font-extrabold text-pink-600">CacheCart</h1>
        <div className="flex items-center gap-6">
          {offline && <WifiOff className="text-red-500" title="Offline Mode" />}
          <Heart className="cursor-pointer text-gray-600 hover:text-pink-500 transition" />
          <div className="relative">
            <ShoppingBag className="cursor-pointer text-gray-600 hover:text-pink-500 transition" />
            {cart.length > 0 && (
              <span className="absolute -top-2 -right-2 bg-pink-500 text-white text-xs px-2 py-0.5 rounded-full">
                {cart.length}
              </span>
            )}
          </div>
        </div>
      </header>

      {/* Banner */}
      <section className="bg-gradient-to-r from-pink-500 to-purple-500 text-white text-center py-16">
        <h2 className="text-5xl font-bold mb-3">Shop the Latest Trends</h2>
        <p className="text-lg opacity-90">Offline-First Shopping, Anytime, Anywhere!</p>
      </section>

      {/* Product Grid */}
      <main className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8 p-8">
        {products.map((product) => (
          <div key={product.id} className="bg-white rounded-2xl shadow hover:shadow-lg transition duration-200">
            <img
              src={product.image}
              alt={product.name}
              className="rounded-t-2xl h-60 w-full object-cover"
            />
            <div className="p-4 text-center">
              <h3 className="font-semibold text-lg text-gray-800">{product.name}</h3>
              <p className="text-gray-600 mb-2">₹{product.price}</p>
              <button
                onClick={() => addToCart(product)}
                className="mt-2 w-full bg-pink-600 hover:bg-pink-700 text-white py-2 rounded-full font-medium transition"
              >
                Add to Cart
              </button>
            </div>
          </div>
        ))}
      </main>

      {/* Footer */}
      <footer className="text-center py-5 bg-white mt-10 shadow-inner text-gray-600 text-sm">
        © 2025 <span className="font-semibold text-pink-600">CacheCart</span> | Offline-First Shopping Experience
      </footer>
    </div>
  );
}
