import React, { useState, useEffect } from 'react';
import { 
  Home, 
  Store, 
  Megaphone, 
  Phone, 
  Mail, 
  MapPin, 
  CreditCard, 
  Upload, 
  CheckCircle, 
  Clock, 
  User, 
  ChevronRight,
  BarChart3,
  Users
} from 'lucide-react';
import { motion, AnimatePresence } from 'framer-motion';

const App = () => {
  const [currentView, setCurrentView] = useState('home'); // 'home', 'merchants', 'marketers', 'ad-detail', 'payment'
  const [selectedAd, setSelectedAd] = useState(null);
  const [paymentStep, setPaymentStep] = useState('init'); // 'init', 'paid', 'receipt-uploaded', 'confirmed'
  const [receiptFile, setReceiptFile] = useState(null);

  // بيانات وهمية للتجار
  const merchants = [
    {
      id: 1,
      title: "عرض خاص على منتجات التجميل الطبيعية",
      description: "منتجات عضوية 100%، توزيع في جميع ولايات الجزائر، هامش ربح جذاب للمسوقين",
      category: "تجميل",
      contact: "+213 555 123 456",
      location: "الجزائر العاصمة",
      price: "500 دج",
      imageUrl: "https://placehold.co/400x250/8B5CF6/FFFFFF?text=تجميل"
    },
    {
      id: 2,
      title: "وكالة تسويق إلكتروني تبحث عن شركاء",
      description: "نساعدك في تسويق منتجاتك عبر منصات التواصل الاجتماعي، خبرة 5 سنوات في السوق الجزائري",
      category: "تسويق",
      contact: "+213 666 789 012",
      location: "وهران",
      price: "1500 دج",
      imageUrl: "https://placehold.co/400x250/10B981/FFFFFF?text=تسويق"
    }
  ];

  // بيانات وهمية للمسوقين
  const marketers = [
    {
      id: 3,
      title: "مسوق محترف - خبرة في مجال الإلكترونيات",
      description: "لدي شبكة علاقات قوية في الجامعات والمدارس، أحقق مبيعات عالية في المنتجات التقنية",
      category: "إلكترونيات",
      contact: "+213 777 321 654",
      location: "قسنطينة",
      price: "400 دج لكل عملية بيع",
      imageUrl: "https://placehold.co/400x250/EF4444/FFFFFF?text=إلكترونيات"
    },
    {
      id: 4,
      title: "فريق تسويق نسائي - تخصص منتجات الأسرة",
      description: "فريق متكامل من 10 مسوقات، متواجدات في 5 ولايات، جاهزات للتعاون الفوري",
      category: "أسرة",
      contact: "+213 555 987 654",
      location: "سطيف",
      price: "300 دج",
      imageUrl: "https://placehold.co/400x250/F59E0B/FFFFFF?text=أسرة"
    }
  ];

  const handleAdClick = (ad) => {
    setSelectedAd(ad);
    setCurrentView('ad-detail');
  };

  const handlePaymentInit = () => {
    setCurrentView('payment');
    setPaymentStep('init');
  };

  const simulateBaridiMobPayment = () => {
    // محاكاة الانتقال إلى تطبيق BaridiMob (لن نفعل رابطًا حقيقيًا)
    alert("سيتم فتح تطبيق BaridiMob تلقائيًا في البيئة الحقيقية.\nالمبلغ: 200 دج\nلتأكيد الدفع: 1234 5678 9012 3456");
    setPaymentStep('paid');
  };

  const handleReceiptUpload = (e) => {
    const file = e.target.files[0];
    if (file) {
      setReceiptFile(file);
      setPaymentStep('receipt-uploaded');
    }
  };

  const confirmPaymentManually = () => {
    // في الواقع، سيقوم المدير بالتأكيد يدويًا
    // هنا نحاكي التأكيد
    setPaymentStep('confirmed');
  };

  const resetFlow = () => {
    setCurrentView('home');
    setSelectedAd(null);
    setPaymentStep('init');
    setReceiptFile(null);
  };

  // مكون بطاقة الإعلان
  const AdCard = ({ ad, onClick }) => (
    <motion.div 
      whileHover={{ y: -5 }}
      whileTap={{ scale: 0.98 }}
      className="bg-white rounded-2xl shadow-lg overflow-hidden cursor-pointer transition-all duration-300 hover:shadow-xl"
      onClick={() => onClick(ad)}
    >
      <div className="h-48 overflow-hidden">
        <img 
          src={ad.imageUrl} 
          alt={ad.title}
          className="w-full h-full object-cover"
        />
      </div>
      <div className="p-5">
        <div className="flex items-center justify-between mb-2">
          <span className="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm font-medium">
            {ad.category}
          </span>
          <span className="text-lg font-bold text-green-600">{ad.price}</span>
        </div>
        <h3 className="text-xl font-bold text-gray-800 mb-2">{ad.title}</h3>
        <p className="text-gray-600 mb-4 line-clamp-2">{ad.description}</p>
        <div className="flex items-center text-gray-500 text-sm mb-3">
          <MapPin size={16} className="ml-1" />
          {ad.location}
        </div>
        <button 
          className="w-full bg-gradient-to-r from-blue-600 to-indigo-700 text-white py-2 rounded-lg font-medium flex items-center justify-center"
        >
          عرض التفاصيل
          <ChevronRight size={18} className="mr-1" />
        </button>
      </div>
    </motion.div>
  );

  // مكون الصفحة الرئيسية
  const HomePage = () => (
    <div className="min-h-screen bg-gradient-to-br from-gray-50 to-blue-50 p-4">
      {/* الهيدر */}
      <header className="max-w-6xl mx-auto py-6">
        <div className="flex items-center justify-between">
          <motion.div 
            initial={{ opacity: 0, x: -20 }}
            animate={{ opacity: 1, x: 0 }}
            className="flex items-center space-x-3 space-x-reverse"
          >
            <div className="bg-blue-600 p-2 rounded-lg">
              <BarChart3 size={32} className="text-white" />
            </div>
            <div>
              <h1 className="text-2xl font-bold text-gray-800">وَسَط</h1>
              <p className="text-gray-600">وسيط موثوق بين التجار والمسوقين في الجزائر</p>
            </div>
          </motion.div>
          <motion.button 
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
            className="flex items-center space-x-2 space-x-reverse bg-white px-4 py-2 rounded-lg shadow-md"
          >
            <User size={20} />
            <span>تسجيل الدخول</span>
          </motion.button>
        </div>
      </header>

      {/* المحتوى الرئيسي */}
      <main className="max-w-6xl mx-auto py-8">
        <motion.div 
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ delay: 0.2 }}
          className="text-center mb-12"
        >
          <h2 className="text-3xl md:text-4xl font-bold text-gray-800 mb-4">
            اربط بين <span className="text-blue-600">التجار</span> و<span className="text-green-600">المسوقين</span>
          </h2>
          <p className="text-gray-600 max-w-2xl mx-auto">
            منصة إلكترونية جزائرية تتيح للتجار نشر عروضهم، وللمسوقين اختيار ما يناسب مهاراتهم، مع ضمان الدفع الآمن عبر بريدي موب
          </p>
        </motion.div>

        {/* البطاقات الرئيسية */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-4xl mx-auto">
          {/* بطاقة التجار */}
          <motion.div 
            whileHover={{ y: -10 }}
            whileTap={{ scale: 0.98 }}
            className="bg-white rounded-3xl shadow-xl overflow-hidden"
          >
            <div className="bg-gradient-to-r from-blue-500 to-indigo-600 p-8 text-white text-center">
              <Store size={48} className="mx-auto mb-4" />
              <h3 className="text-2xl font-bold mb-2">إعلانات التجار</h3>
              <p>انشر عروضك وابحث عن مسوقين أكفاء</p>
            </div>
            <div className="p-6">
              <p className="text-gray-600 mb-4">
                هل لديك منتج أو خدمة وتبحث عن مسوقين محترفين؟ سجل في منصتنا وانشر إعلانك بسهولة.
              </p>
              <button 
                onClick={() => setCurrentView('merchants')}
                className="w-full bg-blue-600 text-white py-3 rounded-lg font-medium flex items-center justify-center"
              >
                عرض الإعلانات
                <ChevronRight size={20} className="mr-2" />
              </button>
            </div>
          </motion.div>

          {/* بطاقة المسوقين */}
          <motion.div 
            whileHover={{ y: -10 }}
            whileTap={{ scale: 0.98 }}
            className="bg-white rounded-3xl shadow-xl overflow-hidden"
          >
            <div className="bg-gradient-to-r from-green-500 to-emerald-600 p-8 text-white text-center">
              <Megaphone size={48} className="mx-auto mb-4" />
              <h3 className="text-2xl font-bold mb-2">إعلانات المسوقين</h3>
              <p>اعرض مهاراتك وابحث عن شركاء تجاريين</p>
            </div>
            <div className="p-6">
              <p className="text-gray-600 mb-4">
                هل أنت مسوق محترف؟ سجّل في المنصة واعرض خدماتك للتجار، وابدأ في تحقيق الدخل.
              </p>
              <button 
                onClick={() => setCurrentView('marketers')}
                className="w-full bg-green-600 text-white py-3 rounded-lg font-medium flex items-center justify-center"
              >
                عرض الإعلانات
                <ChevronRight size={20} className="mr-2" />
              </button>
            </div>
          </motion.div>
        </div>

        {/* مزايا المنصة */}
        <motion.div 
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.5 }}
          className="mt-16 max-w-4xl mx-auto"
        >
          <h3 className="text-2xl font-bold text-center text-gray-800 mb-8">لماذا تختار وَسَط؟</h3>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {[
              { icon: <CreditCard size={28} />, title: "دفع آمن", desc: "تكامل مع بريدي موب لضمان أمان المعاملات المالية" },
              { icon: <Users size={28} />, title: "شبكة واسعة", desc: "وصول إلى آلاف التجار والمسوقين في جميع ولايات الجزائر" },
              { icon: <CheckCircle size={28} />, title: "تأكيد يدوي", desc: "فريق مراقبة يدوي يضمن جودة الإعلانات وصحة الدفع" }
            ].map((item, i) => (
              <div key={i} className="bg-white p-6 rounded-xl shadow-md text-center">
                <div className="inline-block p-3 bg-blue-100 text-blue-600 rounded-lg mb-4">
                  {item.icon}
                </div>
                <h4 className="font-bold text-lg text-gray-800 mb-2">{item.title}</h4>
                <p className="text-gray-600">{item.desc}</p>
              </div>
            ))}
          </div>
        </motion.div>
      </main>

      <footer className="max-w-6xl mx-auto py-8 text-center text-gray-500 text-sm mt-12 border-t">
        © {new Date().getFullYear()} وَسَط - منصة وسيطة جزائرية. جميع الحقوق محفوظة.
      </footer>
    </div>
  );

  // مكون عرض الإعلانات
  const AdsPage = ({ ads, title, icon, backTo }) => (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-6xl mx-auto">
        <button 
          onClick={() => setCurrentView(backTo)}
          className="flex items-center text-blue-600 mb-6 hover:text-blue-800"
        >
          <ChevronRight size={20} className="ml-1 rotate-180" />
          العودة إلى الصفحة الرئيسية
        </button>
        
        <div className="flex items-center space-x-3 space-x-reverse mb-8">
          <div className="bg-blue-100 p-3 rounded-lg text-blue-600">
            {icon}
          </div>
          <h2 className="text-2xl font-bold text-gray-800">{title}</h2>
        </div>

        {ads.length === 0 ? (
          <div className="text-center py-12">
            <div className="bg-gray-200 border-2 border-dashed rounded-xl w-16 h-16 mx-auto mb-4" />
            <p className="text-gray-500">لا توجد إعلانات حالياً</p>
          </div>
        ) : (
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            {ads.map(ad => (
              <AdCard key={ad.id} ad={ad} onClick={handleAdClick} />
            ))}
          </div>
        )}
      </div>
    </div>
  );

  // مكون تفاصيل الإعلان
  const AdDetailPage = () => (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-4xl mx-auto">
        <button 
          onClick={() => setCurrentView(selectedAd.id <= 2 ? 'merchants' : 'marketers')}
          className="flex items-center text-blue-600 mb-6 hover:text-blue-800"
        >
          <ChevronRight size={20} className="ml-1 rotate-180" />
          العودة إلى القائمة
        </button>

        <div className="bg-white rounded-2xl shadow-xl overflow-hidden">
          <div className="h-64 md:h-80 overflow-hidden">
            <img 
              src={selectedAd.imageUrl} 
              alt={selectedAd.title}
              className="w-full h-full object-cover"
            />
          </div>
          
          <div className="p-6 md:p-8">
            <div className="flex flex-wrap items-center justify-between mb-4">
              <span className="px-4 py-1 bg-blue-100 text-blue-700 rounded-full text-sm font-medium">
                {selectedAd.category}
              </span>
              <span className="text-2xl font-bold text-green-600">{selectedAd.price}</span>
            </div>

            <h1 className="text-2xl md:text-3xl font-bold text-gray-800 mb-4">
              {selectedAd.title}
            </h1>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
              <div>
                <h3 className="font-bold text-gray-700 mb-2">الموقع</h3>
                <div className="flex items-center text-gray-600">
                  <MapPin size={18} className="ml-2 text-gray-500" />
                  {selectedAd.location}
                </div>
              </div>
              <div>
                <h3 className="font-bold text-gray-700 mb-2">معلومات الاتصال (بعد الدفع)</h3>
                <div className="space-y-2">
                  <div className="flex items-center text-gray-500">
                    <Phone size={18} className="ml-2" />
                    <span className="text-gray-400">سيتم الكشف بعد الدفع</span>
                  </div>
                  <div className="flex items-center text-gray-500">
                    <Mail size={18} className="ml-2" />
                    <span className="text-gray-400">سيتم الكشف بعد الدفع</span>
                  </div>
                </div>
              </div>
            </div>

            <div className="mb-8">
              <h3 className="font-bold text-gray-700 mb-3">وصف الإعلان</h3>
              <p className="text-gray-600 leading-relaxed">
                {selectedAd.description}
              </p>
            </div>

            <div className="border-t pt-6">
              <motion.button
                whileHover={{ scale: 1.02 }}
                whileTap={{ scale: 0.98 }}
                onClick={handlePaymentInit}
                className="w-full bg-gradient-to-r from-blue-600 to-indigo-700 text-white py-4 rounded-xl font-bold text-lg flex items-center justify-center"
              >
                <CreditCard size={24} className="ml-2" />
                تواصل مع المُعلن (دفع 200 دج)
              </motion.button>
              <p className="text-center text-gray-500 mt-3 text-sm">
                سيتم تفعيل معلومات الاتصال بعد إتمام الدفع وتأكيد الإيصال
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  // مكون الدفع
  const PaymentPage = () => (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-50 p-4">
      <div className="max-w-2xl mx-auto">
        <button 
          onClick={resetFlow}
          className="flex items-center text-gray-600 mb-6 hover:text-gray-800"
        >
          <ChevronRight size={20} className="ml-1 rotate-180" />
          إلغاء العملية
        </button>

        <div className="bg-white rounded-2xl shadow-xl overflow-hidden">
          <div className="bg-gradient-to-r from-blue-600 to-indigo-700 p-6 text-white text-center">
            <CreditCard size={32} className="mx-auto mb-2" />
            <h2 className="text-2xl font-bold">دفع عبر بريدي موب</h2>
            <p className="opacity-90">الدفع الآمن لتفعيل التواصل مع المُعلن</p>
          </div>

          <div className="p-6">
            {/* خطوات الدفع */}
            <div className="flex justify-between mb-8 relative">
              {[
                { step: 'init', label: 'الدفع', icon: CreditCard },
                { step: 'paid', label: 'الإيصال', icon: Upload },
                { step: 'confirmed', label: 'التأكيد', icon: CheckCircle }
              ].map(({ step, label, icon: Icon }, i) => {
                const isActive = paymentStep === step;
                const isCompleted = 
                  (paymentStep === 'paid' && i < 1) || 
                  (paymentStep === 'receipt-uploaded' && i < 2) ||
                  (paymentStep === 'confirmed' && i < 3);
                
                return (
                  <div key={step} className="flex flex-col items-center">
                    <div className={`w-10 h-10 rounded-full flex items-center justify-center mb-2 ${
                      isActive ? 'bg-blue-600 text-white' : 
                      isCompleted ? 'bg-green-500 text-white' : 
                      'bg-gray-200 text-gray-500'
                    }`}>
                      <Icon size={18} />
                    </div>
                    <span className={`text-sm ${
                      isActive ? 'text-blue-600 font-medium' : 
                      isCompleted ? 'text-green-600' : 
                      'text-gray-500'
                    }`}>
                      {label}
                    </span>
                    {i < 2 && (
                      <div className={`absolute top-5 w-1/2 h-1 ${
                        isCompleted ? 'bg-green-500' : 'bg-gray-200'
                      }`} style={{ left: `${(i * 33) + 12}%` }} />
                    )}
                  </div>
                );
              })}
            </div>

            <AnimatePresence mode="wait">
              {paymentStep === 'init' && (
                <motion.div
                  key="init"
                  initial={{ opacity: 0, y: 20 }}
                  animate={{ opacity: 1, y: 0 }}
                  exit={{ opacity: 0, y: -20 }}
                  className="text-center py-8"
                >
                  <div className="bg-blue-50 border border-blue-200 rounded-xl p-6 mb-6">
                    <h3 className="text-xl font-bold text-gray-800 mb-2">تفاصيل الدفع</h3>
                    <p className="text-gray-600 mb-1">المُعلن: <span className="font-medium">{selectedAd.title}</span></p>
                    <p className="text-gray-600">المبلغ: <span className="font-bold text-green-600">200 دج</span></p>
                  </div>
                  
                  <motion.button
                    whileHover={{ scale: 1.03 }}
                    whileTap={{ scale: 0.98 }}
                    onClick={simulateBaridiMobPayment}
                    className="w-full bg-gradient-to-r from-green-500 to-emerald-600 text-white py-4 rounded-xl font-bold text-lg flex items-center justify-center"
                  >
                    <img 
                      src="https://placehold.co/32x32/FFFFFF/000000?text=Baridi" 
                      alt="BaridiMob" 
                      className="ml-2 rounded"
                    />
                    ادفع عبر بريدي موب الآن
                  </motion.button>
                  
                  <p className="text-gray-500 mt-4 text-sm">
                    سيتم فتح تطبيق بريدي موب تلقائيًا. بعد إتمام الدفع، عد إلى هذه الصفحة لرفع قسيمة الدفع.
                  </p>
                </motion.div>
              )}

              {paymentStep === 'paid' && (
                <motion.div
                  key="paid"
                  initial={{ opacity: 0, y: 20 }}
                  animate={{ opacity: 1, y: 0 }}
                  exit={{ opacity: 0, y: -20 }}
                  className="text-center py-8"
                >
                  <div className="bg-green-50 border border-green-200 rounded-xl p-6 mb-6">
                    <CheckCircle size={48} className="mx-auto text-green-500 mb-4" />
                    <h3 className="text-xl font-bold text-gray-800 mb-2">تم الدفع بنجاح!</h3>
                    <p className="text-gray-600">يرجى رفع قسيمة الدفع لتأكيد العملية</p>
                  </div>
                  
                  <div className="border-2 border-dashed border-gray-300 rounded-xl p-8 mb-6">
                    <Upload size={48} className="mx-auto text-gray-400 mb-4" />
                    <input 
                      type="file" 
                      accept="image/*,.pdf"
                      onChange={handleReceiptUpload}
                      className="hidden"
                      id="receipt-upload"
                    />
                    <label 
                      htmlFor="receipt-upload"
                      className="cursor-pointer bg-blue-600 text-white px-6 py-3 rounded-lg font-medium inline-block hover:bg-blue-700"
                    >
                      رفع قسيمة الدفع
                    </label>
                    <p className="text-gray-500 mt-2 text-sm">
                      (صورة أو PDF من قسيمة الدفع من بريدي موب)
                    </p>
                  </div>
                  
                  {receiptFile && (
                    <div className="bg-gray-50 p-4 rounded-lg text-center">
                      <p className="text-green-600 font-medium">
                        ✅ تم رفع الملف: {receiptFile.name}
                      </p>
                      <button 
                        onClick={() => setPaymentStep('receipt-uploaded')}
                        className="mt-3 bg-green-600 text-white px-4 py-2 rounded-lg font-medium"
                      >
                        تأكيد الرفع
                      </button>
                    </div>
                  )}
                </motion.div>
              )}

              {paymentStep === 'receipt-uploaded' && (
                <motion.div
                  key="receipt"
                  initial={{ opacity: 0, y: 20 }}
                  animate={{ opacity: 1, y: 0 }}
                  exit={{ opacity: 0, y: -20 }}
                  className="text-center py-8"
                >
                  <div className="bg-yellow-50 border border-yellow-200 rounded-xl p-6 mb-6">
                    <Clock size={48} className="mx-auto text-yellow-500 mb-4" />
                    <h3 className="text-xl font-bold text-gray-800 mb-2">قيد الانتظار</h3>
                    <p className="text-gray-600">
                      تم رفع قسيمة الدفع. يقوم فريق الإدارة بالتحقق من الدفع يدويًا.
                    </p>
                    <p className="text-gray-500 mt-2 text-sm">
                      مدة التأكيد: 24-48 ساعة عمل
                    </p>
                  </div>
                  
                  <button 
                    onClick={confirmPaymentManually}
                    className="mt-4 bg-blue-600 text-white px-6 py-3 rounded-lg font-medium hover:bg-blue-700"
                  >
                    (تجربة) تأكيد الدفع يدويًا الآن
                  </button>
                </motion.div>
              )}

              {paymentStep === 'confirmed' && (
                <motion.div
                  key="confirmed"
                  initial={{ opacity: 0, y: 20 }}
                  animate={{ opacity: 1, y: 0 }}
                  exit={{ opacity: 0, y: -20 }}
                  className="text-center py-8"
                >
                  <div className="bg-green-50 border border-green-200 rounded-xl p-6 mb-6">
                    <CheckCircle size={48} className="mx-auto text-green-500 mb-4" />
                    <h3 className="text-xl font-bold text-gray-800 mb-2">تم التأكيد!</h3>
                    <p className="text-gray-600">
                      تم التحقق من الدفع بنجاح. يمكنك الآن التواصل مع المُعلن.
                    </p>
                  </div>

                  <div className="bg-white border border-gray-200 rounded-xl p-6 mb-6">
                    <h4 className="font-bold text-lg text-gray-800 mb-4">معلومات الاتصال</h4>
                    <div className="space-y-3 text-right">
                      <div className="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                        <span className="text-gray-600">اسم المُعلن:</span>
                        <span className="font-medium">{selectedAd.title.split(' ')[0]}</span>
                      </div>
                      <div className="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                        <span className="text-gray-600">الهاتف:</span>
                        <span className="font-medium font-mono">{selectedAd.contact}</span>
                      </div>
                      <div className="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                        <span className="text-gray-600">الموقع:</span>
                        <span className="font-medium">{selectedAd.location}</span>
                      </div>
                    </div>
                  </div>

                  <div className="flex flex-col sm:flex-row gap-3">
                    <a 
                      href={`tel:${selectedAd.contact.replace(/\s/g, '')}`}
                      className="flex-1 bg-green-600 text-white py-3 rounded-lg font-medium flex items-center justify-center"
                    >
                      <Phone size={20} className="ml-2" />
                      اتصل الآن
                    </a>
                    <a 
                      href={`mailto:contact@example.com?subject=طلب تعاون - ${selectedAd.title}&body=السلام عليكم، أود التواصل بخصوص إعلانكم: ${selectedAd.title}`}
                      className="flex-1 bg-blue-600 text-white py-3 rounded-lg font-medium flex items-center justify-center"
                    >
                      <Mail size={20} className="ml-2" />
                      أرسل بريدًا
                    </a>
                  </div>

                  <button 
                    onClick={resetFlow}
                    className="mt-4 text-blue-600 hover:text-blue-800 font-medium"
                  >
                    العودة إلى الصفحة الرئيسية
                  </button>
                </motion.div>
              )}
            </AnimatePresence>
          </div>
        </div>
      </div>
    </div>
  );

  // عرض المكونات حسب الحالة
  return (
    <div dir="rtl" className="font-sans">
      {currentView === 'home' && <HomePage />}
      {currentView === 'merchants' && (
        <AdsPage 
          ads={merchants} 
          title="إعلانات التجار" 
          icon={<Store size={28} />} 
          backTo="home" 
        />
      )}
      {currentView === 'marketers' && (
        <AdsPage 
          ads={marketers} 
          title="إعلانات المسوقين" 
          icon={<Megaphone size={28} />} 
          backTo="home" 
        />
      )}
      {currentView === 'ad-detail' && selectedAd && <AdDetailPage />}
      {currentView === 'payment' && <PaymentPage />}
    </div>
  );
};

export default App;
