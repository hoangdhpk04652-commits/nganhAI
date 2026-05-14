import React, { useState, useEffect, useMemo } from 'react';
import { 
  ShoppingBag, 
  Search, 
  X, 
  Plus, 
  Minus, 
  ShoppingCart, 
  ChevronRight,
  Star,
  Menu,
  Smartphone,
  Watch,
  Laptop,
  Shirt
} from 'lucide-react';

const MOCK_PRODUCTS = [
  { id: 1, name: "iPhone 15 Pro Max", price: 29990000, category: "Điện thoại", image: "https://images.unsplash.com/photo-1696446701796-da61225697cc?auto=format&fit=crop&q=80&w=400", rating: 5, description: "Chip A17 Pro mạnh mẽ, camera 48MP." },
  { id: 2, name: "MacBook Air M2", price: 24500000, category: "Laptop", image: "https://images.unsplash.com/photo-1611186871348-b1ec696e52c9?auto=format&fit=crop&q=80&w=400", rating: 4, description: "Thiết kế siêu mỏng, pin lên đến 18 tiếng." },
  { id: 3, name: "Apple Watch Series 9", price: 10490000, category: "Phụ kiện", image: "https://images.unsplash.com/photo-1434493907317-a46b53b81822?auto=format&fit=crop&q=80&w=400", rating: 4, description: "Theo dõi sức khỏe chuyên sâu." },
  { id: 4, name: "Áo Polo Nam Premium", price: 350000, category: "Thời trang", image: "https://images.unsplash.com/photo-1581655353564-df123a1eb820?auto=format&fit=crop&q=80&w=400", rating: 5, description: "Chất vải cotton 100% thoáng mát." },
  { id: 5, name: "Tai nghe Sony WH-1000XM5", price: 6990000, category: "Phụ kiện", image: "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?auto=format&fit=crop&q=80&w=400", rating: 5, description: "Chống ồn đỉnh cao, âm thanh hi-res." },
  { id: 6, name: "Samsung Galaxy S24 Ultra", price: 26500000, category: "Điện thoại", image: "https://images.unsplash.com/photo-1610945415295-d9bbf067e59c?auto=format&fit=crop&q=80&w=400", rating: 5, description: "Bút S-Pen tiện lợi, camera Zoom 100x." },
  { id: 7, name: "Giày Sneaker AF1", price: 2800000, category: "Thời trang", image: "https://images.unsplash.com/photo-1595950653106-6c9ebd614d3a?auto=format&fit=crop&q=80&w=400", rating: 4, description: "Phong cách đường phố năng động." },
  { id: 8, name: "iPad Pro M4", price: 28900000, category: "Laptop", image: "https://images.unsplash.com/photo-1544244015-0df4b3ffc6b0?auto=format&fit=crop&q=80&w=400", rating: 5, description: "Màn hình OLED rực rỡ, mỏng nhẹ nhất." },
];

const CATEGORIES = ["Tất cả", "Điện thoại", "Laptop", "Thời trang", "Phụ kiện"];

export default function App() {
  const [cart, setCart] = useState([]);
  const [isCartOpen, setIsCartOpen] = useState(false);
  const [searchTerm, setSearchTerm] = useState("");
  const [selectedCategory, setSelectedCategory] = useState("Tất cả");
  const [selectedProduct, setSelectedProduct] = useState(null);

  const addToCart = (product) => {
    setCart(prev => {
      const existing = prev.find(item => item.id === product.id);
      if (existing) {
        return prev.map(item => item.id === product.id ? { ...item, quantity: item.quantity + 1 } : item);
      }
      return [...prev, { ...product, quantity: 1 }];
    });
    // Hiệu ứng nhẹ khi thêm
  };

  const removeFromCart = (id) => setCart(prev => prev.filter(item => item.id !== id));
  
  const updateQuantity = (id, delta) => {
    setCart(prev => prev.map(item => {
      if (item.id === id) {
        const newQty = Math.max(1, item.quantity + delta);
        return { ...item, quantity: newQty };
      }
      return item;
    }));
  };

  const cartTotal = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const cartCount = cart.reduce((sum, item) => sum + item.quantity, 0);

  const filteredProducts = useMemo(() => {
    return MOCK_PRODUCTS.filter(p => {
      const matchSearch = p.name.toLowerCase().includes(searchTerm.toLowerCase());
      const matchCategory = selectedCategory === "Tất cả" || p.category === selectedCategory;
      return matchSearch && matchCategory;
    });
  }, [searchTerm, selectedCategory]);

  return (
    <div className="min-h-screen bg-gray-50 text-gray-900 font-sans">
      
      {}
      <header className="sticky top-0 z-40 bg-white shadow-sm border-b border-gray-100">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
          <div className="flex items-center gap-4">
            <div className="lg:hidden p-2 hover:bg-gray-100 rounded-lg cursor-pointer">
              <Menu size={24} />
            </div>
            <div className="flex items-center gap-2 cursor-pointer" onClick={() => {setSelectedCategory("Tất cả"); setSearchTerm("")}}>
              <div className="bg-blue-600 p-2 rounded-xl text-white">
                <ShoppingBag size={24} />
              </div>
              <span className="text-xl font-bold tracking-tight text-blue-600">GEMINI SHOP</span>
            </div>
          </div>

          <div className="hidden md:flex flex-1 max-w-md mx-8">
            <div className="relative w-full">
              <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400" size={18} />
              <input 
                type="text" 
                placeholder="Tìm kiếm sản phẩm..."
                className="w-full bg-gray-100 border-none rounded-full py-2 pl-10 pr-4 focus:ring-2 focus:ring-blue-500 transition-all outline-none"
                value={searchTerm}
                onChange={(e) => setSearchTerm(e.target.value)}
              />
            </div>
          </div>

          <div className="flex items-center gap-4">
            <button 
              onClick={() => setIsCartOpen(true)}
              className="relative p-2 text-gray-600 hover:bg-gray-100 rounded-full transition-colors"
            >
              <ShoppingCart size={24} />
              {cartCount > 0 && (
                <span className="absolute top-0 right-0 bg-red-500 text-white text-[10px] font-bold w-5 h-5 flex items-center justify-center rounded-full animate-bounce">
                  {cartCount}
                </span>
              )}
            </button>
            <div className="hidden sm:block">
              <button className="bg-blue-600 text-white px-5 py-2 rounded-full font-medium hover:bg-blue-700 transition-all shadow-md active:scale-95">
                Đăng nhập
              </button>
            </div>
          </div>
        </div>
      </header>

      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        {}
        <section className="mb-12 relative rounded-3xl overflow-hidden bg-gradient-to-r from-blue-600 to-indigo-700 text-white p-8 sm:p-12 shadow-xl">
          <div className="relative z-10 max-w-xl">
            <span className="inline-block bg-white/20 backdrop-blur-md px-4 py-1 rounded-full text-sm font-semibold mb-4">
              🔥 Khuyến mãi Mùa Hè 2024
            </span>
            <h1 className="text-4xl sm:text-6xl font-extrabold mb-6 leading-tight">
              Nâng Tầm Trải Nghiệm Công Nghệ
            </h1>
            <p className="text-lg text-blue-100 mb-8">
              Giảm ngay tới 50% cho các sản phẩm Apple và Samsung. Hỗ trợ trả góp 0% lãi suất.
            </p>
            <button className="bg-white text-blue-600 px-8 py-4 rounded-xl font-bold flex items-center gap-2 hover:bg-blue-50 transition-all group">
              Mua Ngay <ChevronRight className="group-hover:translate-x-1 transition-transform" />
            </button>
          </div>
          <div className="absolute right-0 top-0 bottom-0 w-1/3 bg-white/10 hidden lg:block skew-x-12 transform origin-right"></div>
        </section>

        {}
        <section className="mb-10 overflow-x-auto no-scrollbar">
          <div className="flex gap-4 pb-2">
            {CATEGORIES.map(cat => (
              <button
                key={cat}
                onClick={() => setSelectedCategory(cat)}
                className={`whitespace-nowrap px-6 py-2 rounded-full font-medium transition-all ${
                  selectedCategory === cat 
                  ? "bg-blue-600 text-white shadow-lg" 
                  : "bg-white text-gray-600 border border-gray-200 hover:border-blue-400"
                }`}
              >
                {cat}
              </button>
            ))}
          </div>
        </section>

        {}
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
          {filteredProducts.map(product => (
            <div 
              key={product.id} 
              className="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 group border border-gray-100 flex flex-col"
            >
              <div 
                className="relative h-64 overflow-hidden cursor-pointer"
                onClick={() => setSelectedProduct(product)}
              >
                <img 
                  src={product.image} 
                  alt={product.name} 
                  className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500"
                />
                <div className="absolute top-3 right-3 bg-white/90 backdrop-blur px-2 py-1 rounded-lg text-xs font-bold text-gray-700">
                  {product.category}
                </div>
              </div>
              <div className="p-5 flex flex-col flex-1">
                <div className="flex items-center gap-1 text-yellow-400 mb-2">
                  {[...Array(5)].map((_, i) => (
                    <Star key={i} size={14} fill={i < product.rating ? "currentColor" : "none"} />
                  ))}
                </div>
                <h3 
                  className="text-lg font-bold text-gray-800 mb-2 group-hover:text-blue-600 transition-colors cursor-pointer line-clamp-1"
                  onClick={() => setSelectedProduct(product)}
                >
                  {product.name}
                </h3>
                <p className="text-sm text-gray-500 mb-4 line-clamp-2">{product.description}</p>
                <div className="mt-auto flex items-center justify-between">
                  <span className="text-xl font-bold text-blue-600">
                    {product.price.toLocaleString()}đ
                  </span>
                  <button 
                    onClick={() => addToCart(product)}
                    className="p-3 bg-gray-50 text-blue-600 rounded-xl hover:bg-blue-600 hover:text-white transition-all active:scale-90"
                  >
                    <Plus size={20} />
                  </button>
                </div>
              </div>
            </div>
          ))}
        </div>

        {filteredProducts.length === 0 && (
          <div className="text-center py-20">
            <div className="text-gray-300 mb-4 flex justify-center">
              <Search size={64} />
            </div>
            <h3 className="text-xl font-semibold text-gray-600">Không tìm thấy sản phẩm nào</h3>
            <p className="text-gray-400">Hãy thử từ khóa khác hoặc xóa bộ lọc.</p>
          </div>
        )}
      </main>

      {}
      {isCartOpen && (
        <div className="fixed inset-0 z-50 overflow-hidden">
          <div className="absolute inset-0 bg-black/40 backdrop-blur-sm transition-opacity" onClick={() => setIsCartOpen(false)} />
          <div className="absolute inset-y-0 right-0 max-w-md w-full bg-white shadow-2xl flex flex-col animate-slide-left">
            <div className="p-6 border-b flex items-center justify-between">
              <h2 className="text-2xl font-bold flex items-center gap-2">
                <ShoppingCart className="text-blue-600" /> Giỏ hàng ({cartCount})
              </h2>
              <button onClick={() => setIsCartOpen(false)} className="p-2 hover:bg-gray-100 rounded-full transition-colors">
                <X size={24} />
              </button>
            </div>

            <div className="flex-1 overflow-y-auto p-6 space-y-6">
              {cart.length === 0 ? (
                <div className="h-full flex flex-col items-center justify-center text-gray-400">
                  <ShoppingBag size={80} strokeWidth={1} className="mb-4" />
                  <p className="text-lg">Giỏ hàng của bạn đang trống</p>
                  <button 
                    onClick={() => setIsCartOpen(false)}
                    className="mt-4 text-blue-600 font-semibold hover:underline"
                  >
                    Tiếp tục mua sắm
                  </button>
                </div>
              ) : (
                cart.map(item => (
                  <div key={item.id} className="flex gap-4 group">
                    <img src={item.image} className="w-20 h-20 rounded-xl object-cover border border-gray-100" />
                    <div className="flex-1">
                      <div className="flex justify-between">
                        <h4 className="font-bold text-gray-800">{item.name}</h4>
                        <button onClick={() => removeFromCart(item.id)} className="text-gray-300 hover:text-red-500 transition-colors">
                          <X size={18} />
                        </button>
                      </div>
                      <p className="text-blue-600 font-semibold mb-3">{item.price.toLocaleString()}đ</p>
                      <div className="flex items-center gap-3">
                        <div className="flex items-center border border-gray-200 rounded-lg overflow-hidden">
                          <button 
                            onClick={() => updateQuantity(item.id, -1)}
                            className="p-1 px-2 hover:bg-gray-100 transition-colors"
                          >
                            <Minus size={14} />
                          </button>
                          <span className="w-8 text-center text-sm font-bold">{item.quantity}</span>
                          <button 
                            onClick={() => updateQuantity(item.id, 1)}
                            className="p-1 px-2 hover:bg-gray-100 transition-colors"
                          >
                            <Plus size={14} />
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                ))
              )}
            </div>

            {cart.length > 0 && (
              <div className="p-6 border-t bg-gray-50">
                <div className="flex justify-between mb-4">
                  <span className="text-gray-600">Tạm tính</span>
                  <span className="text-xl font-bold text-gray-900">{cartTotal.toLocaleString()}đ</span>
                </div>
                <button className="w-full bg-blue-600 text-white py-4 rounded-xl font-bold hover:bg-blue-700 transition-all shadow-lg active:scale-[0.98]">
                  Thanh Toán Ngay
                </button>
                <p className="text-center text-xs text-gray-400 mt-4">Miễn phí vận chuyển cho đơn hàng từ 500k</p>
              </div>
            )}
          </div>
        </div>
      )}

      {}
      {selectedProduct && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
          <div className="absolute inset-0 bg-black/60 backdrop-blur-md" onClick={() => setSelectedProduct(null)} />
          <div className="relative bg-white w-full max-w-4xl rounded-3xl overflow-hidden shadow-2xl flex flex-col md:flex-row animate-scale-up">
            <button 
              onClick={() => setSelectedProduct(null)}
              className="absolute top-4 right-4 z-10 p-2 bg-white/80 hover:bg-white rounded-full transition-colors shadow-sm"
            >
              <X size={20} />
            </button>
            
            <div className="md:w-1/2 h-80 md:h-auto overflow-hidden">
              <img src={selectedProduct.image} className="w-full h-full object-cover" />
            </div>
            
            <div className="p-8 md:w-1/2 flex flex-col">
              <div className="flex items-center gap-2 mb-4">
                <span className="bg-blue-100 text-blue-600 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider">
                  {selectedProduct.category}
                </span>
                <div className="flex items-center gap-1 text-yellow-400">
                  <Star size={14} fill="currentColor" />
                  <span className="text-sm font-bold text-gray-700">{selectedProduct.rating}.0</span>
                </div>
              </div>
              
              <h2 className="text-3xl font-extrabold text-gray-900 mb-4">{selectedProduct.name}</h2>
              <p className="text-gray-500 mb-6 leading-relaxed">
                {selectedProduct.description}. Đây là sản phẩm chính hãng với chế độ bảo hành lên tới 12 tháng tại hệ thống Gemini Shop trên toàn quốc. 
              </p>
              
              <div className="mt-auto">
                <div className="text-4xl font-black text-blue-600 mb-8">
                  {selectedProduct.price.toLocaleString()}đ
                </div>
                <div className="flex gap-4">
                  <button 
                    onClick={() => {
                      addToCart(selectedProduct);
                      setSelectedProduct(null);
                      setIsCartOpen(true);
                    }}
                    className="flex-1 bg-blue-600 text-white py-4 rounded-2xl font-bold flex items-center justify-center gap-2 hover:bg-blue-700 transition-all active:scale-95 shadow-lg shadow-blue-200"
                  >
                    <ShoppingCart size={20} /> Thêm vào giỏ
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      )}

      {}
      <footer className="bg-white border-t border-gray-100 py-12 mt-20">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="grid grid-cols-1 md:grid-cols-4 gap-12">
            <div className="col-span-1 md:col-span-1">
              <div className="flex items-center gap-2 mb-6">
                <div className="bg-blue-600 p-2 rounded-xl text-white">
                  <ShoppingBag size={20} />
                </div>
                <span className="text-xl font-bold text-blue-600">GEMINI SHOP</span>
              </div>
              <p className="text-gray-500 text-sm leading-relaxed">
                Chuyên cung cấp các thiết bị công nghệ và thời trang cao cấp chính hãng. Uy tín - Chất lượng - Tận tâm.
              </p>
            </div>
            <div>
              <h4 className="font-bold text-gray-900 mb-6 uppercase text-xs tracking-widest">Sản Phẩm</h4>
              <ul className="space-y-4 text-gray-500 text-sm">
                <li><a href="#" className="hover:text-blue-600 transition-colors">Điện thoại mới</a></li>
                <li><a href="#" className="hover:text-blue-600 transition-colors">Laptop Gaming</a></li>
                <li><a href="#" className="hover:text-blue-600 transition-colors">Phụ kiện công nghệ</a></li>
                <li><a href="#" className="hover:text-blue-600 transition-colors">Thời trang nam/nữ</a></li>
              </ul>
            </div>
            <div>
              <h4 className="font-bold text-gray-900 mb-6 uppercase text-xs tracking-widest">Hỗ Trợ</h4>
              <ul className="space-y-4 text-gray-500 text-sm">
                <li><a href="#" className="hover:text-blue-600 transition-colors">Chính sách bảo hành</a></li>
                <li><a href="#" className="hover:text-blue-600 transition-colors">Đổi trả trong 30 ngày</a></li>
                <li><a href="#" className="hover:text-blue-600 transition-colors">Phương thức thanh toán</a></li>
                <li><a href="#" className="hover:text-blue-600 transition-colors">Giao hàng & COD</a></li>
              </ul>
            </div>
            <div>
              <h4 className="font-bold text-gray-900 mb-6 uppercase text-xs tracking-widest">Liên Hệ</h4>
              <div className="flex flex-col space-y-4">
                <p className="text-sm text-gray-500">📍 123 Đường Công Nghệ, Quận 1, TP.HCM</p>
                <p className="text-sm text-gray-500">📞 Hotline: 1800 6789</p>
                <div className="flex gap-4 pt-4">
                  <div className="w-10 h-10 bg-gray-100 rounded-full flex items-center justify-center text-gray-400 hover:bg-blue-600 hover:text-white transition-all cursor-pointer">
                    f
                  </div>
                  <div className="w-10 h-10 bg-gray-100 rounded-full flex items-center justify-center text-gray-400 hover:bg-blue-600 hover:text-white transition-all cursor-pointer">
                    t
                  </div>
                  <div className="w-10 h-10 bg-gray-100 rounded-full flex items-center justify-center text-gray-400 hover:bg-blue-600 hover:text-white transition-all cursor-pointer">
                    i
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div className="border-t border-gray-100 mt-12 pt-8 text-center text-gray-400 text-xs">
            © 2024 Gemini Shop. All rights reserved. Designed by Gemini AI.
          </div>
        </div>
      </footer>

      <style dangerouslySetInnerHTML={{ __html: `
        @keyframes slide-left {
          from { transform: translateX(100%); }
          to { transform: translateX(0); }
        }
        @keyframes scale-up {
          from { opacity: 0; transform: scale(0.95); }
          to { opacity: 1; transform: scale(1); }
        }
        .animate-slide-left { animation: slide-left 0.3s ease-out; }
        .animate-scale-up { animation: scale-up 0.2s ease-out; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
      `}} />

    </div>
  );
}
