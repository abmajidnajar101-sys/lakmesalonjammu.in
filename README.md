# lakmesalonjammu.inimport React, { useState, useEffect, useRef } from 'react';
import { motion, AnimatePresence, useScroll, useTransform, useSpring } from 'framer-motion';
import { 
  Phone, 
  MessageCircle, 
  MapPin, 
  Star, 
  Clock, 
  Scissors, 
  Sparkles, 
  Heart, 
  ChevronRight, 
  ChevronLeft,
  Navigation,
  CheckCircle,
  Menu,
  X,
  Volume2,
  VolumeX,
  Send,
  Instagram,
  Facebook
} from 'lucide-react';

// --- Components ---

const Navbar = () => {
  const [scrolled, setScrolled] = useState(false);
  const [isOpen, setIsOpen] = useState(false);

  useEffect(() => {
    const handleScroll = () => setScrolled(window.scrollY > 50);
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <nav className={`fixed w-full z-[100] transition-all duration-500 ${scrolled ? 'bg-white/70 backdrop-blur-xl py-3 shadow-lg' : 'bg-transparent py-6'}`}>
      <div className="max-w-7xl mx-auto px-6 flex justify-between items-center">
        <div className="flex flex-col">
          <span className="text-2xl font-bold tracking-tighter text-slate-900 font-serif">LAKMÉ <span className="text-pink-600">SALON</span></span>
          <span className="text-[10px] tracking-[0.2em] uppercase font-medium text-slate-500">Trikuta Nagar, Jammu</span>
        </div>

        <div className="hidden md:flex items-center gap-8 text-sm font-medium uppercase tracking-widest text-slate-700">
          <a href="#services" className="hover:text-pink-600 transition-colors">Services</a>
          <a href="#gallery" className="hover:text-pink-600 transition-colors">Gallery</a>
          <a href="#reviews" className="hover:text-pink-600 transition-colors">Reviews</a>
          <a href="#offers" className="bg-pink-600 text-white px-6 py-2 rounded-full shadow-lg hover:shadow-pink-200 transition-all">Book Now</a>
        </div>

        <button className="md:hidden p-2 text-slate-900" onClick={() => setIsOpen(!isOpen)}>
          {isOpen ? <X size={28} /> : <Menu size={28} />}
        </button>
      </div>

      <AnimatePresence>
        {isOpen && (
          <motion.div 
            initial={{ opacity: 0, y: -20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }}
            className="absolute top-full left-0 w-full bg-white/95 backdrop-blur-2xl border-b border-pink-100 p-8 flex flex-col gap-6 md:hidden"
          >
            <a href="#services" onClick={() => setIsOpen(false)}>Services</a>
            <a href="#gallery" onClick={() => setIsOpen(false)}>Gallery</a>
            <a href="#reviews" onClick={() => setIsOpen(false)}>Reviews</a>
            <a href="tel:07006622703" className="text-pink-600 font-bold">Call Now</a>
          </motion.div>
        )}
      </AnimatePresence>
    </nav>
  );
};

const FloatingBeautyElements = () => (
  <div className="absolute inset-0 pointer-events-none overflow-hidden z-0">
    <motion.div animate={{ y: [0, -40, 0], rotate: [0, 10, 0] }} transition={{ duration: 6, repeat: Infinity }} className="absolute top-[20%] left-[10%] opacity-20 text-pink-400">
      <Scissors size={120} />
    </motion.div>
    <motion.div animate={{ y: [0, 30, 0], rotate: [0, -15, 0] }} transition={{ duration: 5, repeat: Infinity, delay: 1 }} className="absolute bottom-[20%] right-[15%] opacity-10 text-amber-500">
      <Sparkles size={160} />
    </motion.div>
    <motion.div animate={{ scale: [1, 1.2, 1] }} transition={{ duration: 4, repeat: Infinity }} className="absolute top-[60%] left-[5%] opacity-20 text-pink-200">
      <Heart size={80} fill="currentColor" />
    </motion.div>
  </div>
);

const ServiceCard = ({ icon, title, desc }) => {
  const cardRef = useRef(null);
  const [rotate, setRotate] = useState({ x: 0, y: 0 });

  const handleMouseMove = (e) => {
    if (!cardRef.current) return;
    const rect = cardRef.current.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    const multiplier = 10;
    const xc = rect.width / 2;
    const yc = rect.height / 2;
    const dx = (xc - x) / multiplier;
    const dy = (y - yc) / multiplier;
    setRotate({ x: dy, y: dx });
  };

  const handleMouseLeave = () => setRotate({ x: 0, y: 0 });

  return (
    <motion.div
      ref={cardRef}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
      animate={{ rotateX: rotate.x, rotateY: rotate.y }}
      whileHover={{ scale: 1.05 }}
      className="relative p-8 rounded-[2rem] bg-white/40 backdrop-blur-lg border border-white/60 shadow-xl group cursor-pointer overflow-hidden"
      style={{ perspective: 1000 }}
    >
      <div className="absolute top-0 right-0 w-32 h-32 bg-pink-100/50 rounded-full -mr-16 -mt-16 blur-3xl group-hover:bg-pink-200 transition-colors" />
      <div className="text-4xl mb-4 group-hover:scale-110 transition-transform">{icon}</div>
      <h3 className="text-xl font-bold text-slate-800 mb-2 font-serif">{title}</h3>
      <p className="text-slate-500 text-sm leading-relaxed mb-6">{desc}</p>
      <div className="flex items-center gap-2 text-pink-600 text-xs font-bold uppercase tracking-widest">
        Book Session <ChevronRight size={14} />
      </div>
    </motion.div>
  );
};

const BeforeAfterSlider = () => {
  const [position, setPosition] = useState(50);
  const isResizing = useRef(false);

  const handleMove = (e) => {
    if (!isResizing.current) return;
    const rect = e.currentTarget.getBoundingClientRect();
    const x = Math.max(0, Math.min(e.clientX - rect.left, rect.width));
    setPosition((x / rect.width) * 100);
  };

  return (
    <div 
      className="relative w-full aspect-video rounded-[2.5rem] overflow-hidden shadow-2xl cursor-col-resize select-none"
      onMouseMove={handleMove}
      onMouseDown={() => (isResizing.current = true)}
      onMouseUp={() => (isResizing.current = false)}
      onMouseLeave={() => (isResizing.current = false)}
    >
      <div className="absolute inset-0 bg-slate-300 flex items-center justify-center text-slate-500 font-bold uppercase tracking-widest text-xl">After Treatment</div>
      <div className="absolute top-0 left-0 h-full bg-slate-400 z-10 overflow-hidden" style={{ width: `${position}%` }}>
        <div className="w-[1000px] h-full flex items-center justify-center text-white font-bold uppercase tracking-widest text-xl bg-slate-500">Before Treatment</div>
      </div>
      <div className="absolute top-0 bottom-0 z-20 w-1 bg-white" style={{ left: `${position}%` }}>
        <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-10 h-10 bg-white rounded-full shadow-lg flex items-center justify-center">
          <div className="flex gap-1 text-slate-400">
            <ChevronLeft size={16} />
            <ChevronRight size={16} />
          </div>
        </div>
      </div>
    </div>
  );
};

const ChatBot = () => {
  const [isOpen, setIsOpen] = useState(false);
  return (
    <div className="fixed bottom-24 left-6 z-[90]">
      <AnimatePresence>
        {isOpen && (
          <motion.div 
            initial={{ opacity: 0, scale: 0.8, y: 20 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.8, y: 20 }}
            className="w-80 h-96 bg-white rounded-3xl shadow-2xl border border-pink-100 overflow-hidden flex flex-col mb-4"
          >
            <div className="p-4 bg-pink-600 text-white flex justify-between items-center">
              <span className="font-bold flex items-center gap-2"><Sparkles size={16}/> Beauty Bot</span>
              <button onClick={() => setIsOpen(false)}><X size={18}/></button>
            </div>
            <div className="flex-1 p-4 bg-slate-50 overflow-y-auto space-y-4">
              <div className="bg-white p-3 rounded-2xl text-xs text-slate-600 shadow-sm max-w-[80%]">
                Hello! ✨ How can I help you sparkle today?
              </div>
            </div>
            <div className="p-3 border-t flex gap-2">
              <input placeholder="Type your message..." className="flex-1 text-xs outline-none bg-slate-100 rounded-full px-4 py-2" />
              <button className="bg-pink-600 text-white p-2 rounded-full"><Send size={14}/></button>
            </div>
          </motion.div>
        )}
      </AnimatePresence>
      <button 
        onClick={() => setIsOpen(!isOpen)}
        className="bg-white/90 backdrop-blur-md p-4 rounded-full shadow-2xl border border-pink-200 text-pink-600 hover:scale-110 transition-transform"
      >
        <Sparkles size={28} />
      </button>
    </div>
  );
};

const ReviewSlider = () => {
  const reviews = [
    { name: "Priya Sharma", text: "Best hair spa experience! Staff is polite and professional. My hair feels amazing.", rating: 5 },
    { name: "Anjali Gupta", text: "They transformed my look beyond imagination! The bridal package was perfect.", rating: 5 },
    { name: "Sonia Mehra", text: "Haircut and eye massage was so relaxing! The 3D ambiance is very high-end.", rating: 5 }
  ];
  const [idx, setIdx] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => setIdx(prev => (prev + 1) % reviews.length), 5000);
    return () => clearInterval(timer);
  }, []);

  return (
    <div className="relative h-64 flex flex-col items-center justify-center text-center">
      <AnimatePresence mode="wait">
        <motion.div
          key={idx}
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: -20 }}
          className="max-w-2xl px-6"
        >
          <div className="flex justify-center gap-1 mb-4 text-amber-500 drop-shadow-[0_0_8px_rgba(245,158,11,0.5)]">
            {[...Array(5)].map((_, i) => <Star key={i} fill="currentColor" size={24} />)}
          </div>
          <p className="text-xl md:text-2xl font-serif italic text-slate-700 mb-6 leading-relaxed">"{reviews[idx].text}"</p>
          <p className="font-bold text-pink-600 tracking-widest uppercase text-sm">— {reviews[idx].name}</p>
        </motion.div>
      </AnimatePresence>
    </div>
  );
};

const App = () => {
  const [muted, setMuted] = useState(true);
  const { scrollYProgress } = useScroll();
  const scaleX = useSpring(scrollYProgress, { stiffness: 100, damping: 30 });

  return (
    <div className="bg-[#fff9f9] text-slate-800 font-sans overflow-x-hidden selection:bg-pink-100 selection:text-pink-600">
      {/* Scroll Progress */}
      <motion.div className="fixed top-0 left-0 right-0 h-1 bg-gradient-to-r from-pink-500 to-amber-500 origin-left z-[110]" style={{ scaleX }} />

      <Navbar />

      {/* Music Toggle */}
      <button 
        onClick={() => setMuted(!muted)}
        className="fixed top-24 right-6 z-[90] bg-white/50 backdrop-blur-md p-3 rounded-full border border-white/80 text-slate-500"
      >
        {muted ? <VolumeX size={20} /> : <Volume2 size={20} />}
      </button>

      {/* Section 1: Hero */}
      <section className="relative min-h-screen flex items-center justify-center pt-20 overflow-hidden">
        {/* Animated Gradient Background */}
        <div className="absolute inset-0 bg-[radial-gradient(circle_at_50%_50%,#fff5f7_0%,#fff9f9_100%)] z-0" />
        <motion.div 
          animate={{ rotate: 360 }} 
          transition={{ duration: 40, repeat: Infinity, ease: "linear" }}
          className="absolute -top-[20%] -left-[10%] w-[80vw] h-[80vw] bg-pink-100/30 blur-[150px] rounded-full" 
        />
        <motion.div 
          animate={{ rotate: -360 }} 
          transition={{ duration: 50, repeat: Infinity, ease: "linear" }}
          className="absolute -bottom-[20%] -right-[10%] w-[60vw] h-[60vw] bg-amber-100/20 blur-[150px] rounded-full" 
        />

        <FloatingBeautyElements />

        <div className="max-w-7xl mx-auto px-6 relative z-10 text-center">
          <motion.div
            initial={{ opacity: 0, y: 30 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 1, ease: "easeOut" }}
          >
            <div className="flex items-center justify-center gap-3 mb-6">
              <div className="flex text-amber-500">
                {[...Array(5)].map((_, i) => <Star key={i} fill="currentColor" size={16} />)}
              </div>
              <span className="text-xs font-bold tracking-[0.3em] uppercase text-slate-500">4.9 Rated Experience</span>
            </div>

            <h1 className="text-6xl md:text-9xl font-serif font-black leading-[1.1] text-slate-900 mb-8">
              Glow Like <br /> 
              <span className="text-transparent bg-clip-text bg-gradient-to-r from-pink-600 via-rose-500 to-amber-500 animate-gradient">Never Before ✨</span>
            </h1>

            <p className="text-lg md:text-xl text-slate-500 max-w-2xl mx-auto font-light leading-relaxed mb-12">
              Step into the world of Lakmé Salon, Trikuta Nagar. Where Jammu's elite experience 
              unmatched luxury, expert transformations, and pure elegance.
            </p>

            <div className="flex flex-col sm:flex-row items-center justify-center gap-6">
              <motion.a 
                href="tel:07006622703"
                whileHover={{ scale: 1.05, boxShadow: "0 20px 40px rgba(219, 39, 119, 0.2)" }}
                whileTap={{ scale: 0.95 }}
                className="group relative bg-pink-600 text-white px-10 py-5 rounded-full font-bold text-lg overflow-hidden flex items-center gap-2"
              >
                <div className="absolute inset-0 bg-white/20 -translate-x-full group-hover:translate-x-full transition-transform duration-700 skew-x-12" />
                <Phone size={20} /> Call Now
              </motion.a>
              <motion.a 
                href="https://wa.me/917006622703"
                whileHover={{ scale: 1.05 }}
                className="bg-white border-2 border-slate-100 px-10 py-5 rounded-full font-bold text-lg text-slate-800 shadow-xl hover:shadow-slate-200 transition-all flex items-center gap-2"
              >
                <MessageCircle size={20} className="text-green-500" /> WhatsApp Booking
              </motion.a>
            </div>
          </motion.div>
        </div>
      </section>

      {/* Section 2: Services */}
      <section id="services" className="py-24 px-6 relative">
        <div className="max-w-7xl mx-auto">
          <div className="flex flex-col md:flex-row md:items-end justify-between mb-16 gap-6">
            <div>
              <span className="text-pink-600 font-bold tracking-widest uppercase text-xs mb-4 block">Our Expertise</span>
              <h2 className="text-4xl md:text-6xl font-serif font-bold text-slate-900 leading-tight">Elite Beauty Menu</h2>
            </div>
            <p className="text-slate-500 max-w-sm text-sm">Discover our high-definition services tailored for your unique style and skin requirements.</p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <ServiceCard icon="💇‍♀️" title="Hair Cut & Styling" desc="Precision cuts and avant-garde styling by our expert lakmé stylists." />
            <ServiceCard icon="🎨" title="Global Hair Coloring" desc="Bespoke coloring using high-end L'Oréal & Lakmé pigments for depth and shine." />
            <ServiceCard icon="💆" title="Hair Spa & Highlights" desc="Deep conditioning treatments that revive hair texture and health instantly." />
            <ServiceCard icon="👰" title="Bridal Makeup" desc="Luxury bridal packages including HD and Airbrush makeup for your big day." />
            <ServiceCard icon="✨" title="Skin Care & Facial" desc="Advanced medi-facials and glow-enhancing treatments for a radiant look." />
            <ServiceCard icon="💅" title="Manicure & Pedicure" desc="Relaxing nail spa sessions featuring luxury scrub and artisan nail art." />
          </div>
        </div>
      </section>

      {/* Section 3: Google Reviews */}
      <section id="reviews" className="py-24 bg-slate-900 text-white relative overflow-hidden">
        <div className="absolute top-0 right-0 w-96 h-96 bg-pink-500/10 blur-[120px] rounded-full" />
        <div className="max-w-7xl mx-auto px-6 relative z-10">
          <div className="flex flex-col items-center mb-16 text-center">
            <div className="bg-white/10 backdrop-blur-md px-6 py-2 rounded-full border border-white/20 flex items-center gap-3 mb-8">
              <img src="https://upload.wikimedia.org/wikipedia/commons/c/c1/Google_%22G%22_logo.svg" className="w-5 h-5" alt="Google" />
              <span className="text-xs font-bold tracking-widest uppercase">Verified Google Reviews</span>
            </div>
            <h2 className="text-4xl md:text-6xl font-serif font-bold mb-6 italic">2,232 Happy Hearts</h2>
          </div>
          
          <ReviewSlider />

          <div className="flex justify-center gap-12 mt-16 pt-16 border-t border-white/5">
            <div className="text-center">
              <p className="text-3xl font-bold font-serif mb-1">4.9</p>
              <p className="text-[10px] uppercase tracking-widest text-slate-400">Avg Rating</p>
            </div>
            <div className="text-center">
              <p className="text-3xl font-bold font-serif mb-1">100%</p>
              <p className="text-[10px] uppercase tracking-widest text-slate-400">Satisfaction</p>
            </div>
            <div className="text-center">
              <p className="text-3xl font-bold font-serif mb-1">Jammu</p>
              <p className="text-[10px] uppercase tracking-widest text-slate-400">Best Rated</p>
            </div>
          </div>
        </div>
      </section>

      {/* Section 4: Before & After (Interactive) */}
      <section id="gallery" className="py-24 px-6">
        <div className="max-w-7xl mx-auto">
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-16 items-center">
            <div>
              <span className="text-pink-600 font-bold tracking-widest uppercase text-xs mb-4 block">Transformation Gallery</span>
              <h2 className="text-4xl md:text-6xl font-serif font-bold text-slate-900 leading-tight mb-8">Seeing is Believing</h2>
              <p className="text-slate-500 text-lg mb-8 leading-relaxed">
                Experience the magic of our stylists. Use the slider to view our recent hair and 
                skin transformations at Trikuta Nagar. We don't just treat, we transform.
              </p>
              <div className="space-y-4">
                <div className="flex items-center gap-3 text-slate-700 font-medium">
                  <CheckCircle className="text-pink-600" size={20} /> No filters, just expert care.
                </div>
                <div className="flex items-center gap-3 text-slate-700 font-medium">
                  <CheckCircle className="text-pink-600" size={20} /> Professional photography setup.
                </div>
                <div className="flex items-center gap-3 text-slate-700 font-medium">
                  <CheckCircle className="text-pink-600" size={20} /> Verified by real clients.
                </div>
              </div>
            </div>
            <BeforeAfterSlider />
          </div>
        </div>
      </section>

      {/* Section 5: Offers */}
      <section id="offers" className="py-24 px-6 relative">
        <div className="max-w-5xl mx-auto">
          <motion.div 
            initial={{ scale: 0.9, opacity: 0 }}
            whileInView={{ scale: 1, opacity: 1 }}
            className="relative p-12 md:p-20 rounded-[4rem] bg-gradient-to-br from-pink-600 via-rose-500 to-amber-500 text-white overflow-hidden shadow-2xl"
          >
            {/* Animated Hearts */}
            {[...Array(15)].map((_, i) => (
              <motion.div 
                key={i}
                animate={{ 
                  y: [-20, -100], 
                  x: [0, (i % 2 === 0 ? 30 : -30)], 
                  opac
