import React, { useState, createContext, useContext, useEffect } from 'react';
import { HashRouter, Routes, Route, Link, useLocation } from 'react-router-dom';
import { Locale } from './types';
import { DICTIONARIES } from './i18n';

// Pages
import Home from './pages/Home';
import About from './pages/About';
import CertificationPage from './pages/Certification';
import Verify from './pages/Verify';
import Support from './pages/Support';
import Casting from './pages/Casting';
import Membership from './pages/Membership';
import Partners from './pages/Partners';
import Events from './pages/Events';
import Contact from './pages/Contact';
import PolicyPage from './pages/PolicyPage';

// Governance Pages
import Governance from './pages/Governance';
import Standards from './pages/Standards';
import Transparency from './pages/Transparency';
import Protection from './pages/Protection';
import Directory from './pages/Directory';
import Reporting from './pages/Reporting';

const LanguageContext = createContext({
  locale: Locale.EN,
  setLocale: (l: Locale) => {},
  t: DICTIONARIES[Locale.EN]
});

export const useLocale = () => useContext(LanguageContext);

const Navbar = () => {
  const { locale, setLocale, t } = useLocale();
  const [isOpen, setIsOpen] = useState(false);
  const [isCertOpen, setIsCertOpen] = useState(false);
  const { pathname } = useLocation();

  const links = [
    { path: '/about', label: t.nav.about },
    { path: '/support', label: t.nav.support },
    { path: '/casting', label: t.nav.casting },
    { path: '/membership', label: t.nav.membership },
    { path: '/partners', label: t.nav.partners },
    { path: '/events', label: t.nav.events },
  ];

  const certGovLinks = [
    { path: '/certification', label: t.nav.certGov.overview },
    { path: '/verify', label: t.nav.certGov.verify },
    { path: '/governance', label: t.nav.certGov.governance },
    { path: '/standards', label: t.nav.certGov.standards },
    { path: '/transparency', label: t.nav.certGov.transparency },
    { path: '/protection', label: t.nav.certGov.protection },
    { path: '/directory', label: t.nav.certGov.directory },
    { path: '/reporting', label: t.nav.certGov.reporting },
  ];

  const languages = [
    { code: Locale.EN, label: t.language.en },
    { code: Locale.ZH, label: t.language.zh },
    { code: Locale.ES, label: t.language.es },
    { code: Locale.FR, label: t.language.fr },
    { code: Locale.IT, label: t.language.it },
  ];

  useEffect(() => {
    setIsOpen(false);
    window.scrollTo(0, 0);
  }, [pathname]);

  return (
    <nav className="fixed w-full z-50 bg-gfa-black/95 backdrop-blur-xl border-b border-gfa-gold/20">
      <div className="max-w-7xl mx-auto px-4">
        <div className="flex justify-between items-center h-20">
          <Link to="/" className="flex items-center gap-3 group">
            <img 
              src="https://i.ibb.co/B582n2Dk/1755827874220993959.png" 
              alt="GFA Logo" 
              className="h-10 w-auto object-contain transition-transform group-hover:scale-105"
            />
            <div className="flex flex-col">
              <span className="text-lg font-black gold-gradient tracking-tighter leading-none">GFA</span>
              <span className="text-[5.5px] tracking-[0.4em] text-gfa-gray uppercase font-bold">Global Film Alliance</span>
            </div>
          </Link>

          <div className="hidden lg:flex items-center space-x-5">
            <Link to="/about" className={`text-[10px] font-bold uppercase tracking-widest transition-colors hover:text-gfa-gold ${pathname === '/about' ? 'text-gfa-gold' : 'text-gfa-gray'}`}>
              {t.nav.about}
            </Link>
            
            <div className="relative group" onMouseEnter={() => setIsCertOpen(true)} onMouseLeave={() => setIsCertOpen(false)}>
              <button className={`text-[10px] font-bold uppercase tracking-widest flex items-center gap-1 transition-colors ${certGovLinks.some(l => pathname === l.path) ? 'text-gfa-gold' : 'text-gfa-gray hover:text-gfa-gold'}`}>
                {t.nav.certification} <svg className="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path d="M19 9l-7 7-7-7" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"/></svg>
              </button>
              {isCertOpen && (
                <div className="absolute top-full left-0 bg-gfa-darkGray border border-gfa-gold/20 py-4 w-64 shadow-2xl animate-fade-in">
                  {certGovLinks.map(gl => (
                    <Link key={gl.path} to={gl.path} className="block px-6 py-2 text-[10px] font-bold uppercase tracking-widest text-gfa-gray hover:text-gfa-gold hover:bg-white/5 transition-colors">
                      {gl.label}
                    </Link>
                  ))}
                </div>
              )}
            </div>

            {links.slice(1).map(l => (
              <Link key={l.path} to={l.path} className={`text-[10px] font-bold uppercase tracking-widest transition-colors hover:text-gfa-gold ${pathname === l.path ? 'text-gfa-gold' : 'text-gfa-gray'}`}>
                {l.label}
              </Link>
            ))}
          </div>

          <div className="flex items-center space-x-4">
            <select 
              value={locale} 
              onChange={(e) => setLocale(e.target.value as Locale)}
              className="bg-transparent border border-gfa-gold/30 text-[10px] text-gfa-gold font-bold px-2 py-1 focus:outline-none cursor-pointer rounded-sm"
            >
              {languages.map(lang => <option key={lang.code} value={lang.code} className="bg-gfa-black">{lang.label}</option>)}
            </select>
            <button className="lg:hidden text-gfa-gray p-2" onClick={() => setIsOpen(!isOpen)}>
              <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d={isOpen ? "M6 18L18 6M6 6l12 12" : "M4 6h16M4 12h16m-7 6h7"} /></svg>
            </button>
          </div>
        </div>
      </div>
      
      {isOpen && (
        <div className="lg:hidden fixed inset-0 top-20 bg-gfa-black z-40 overflow-y-auto animate-fade-in">
          <div className="p-4 space-y-2">
            <Link to="/about" className="block px-6 py-4 text-sm font-bold uppercase tracking-widest text-gfa-gray hover:text-gfa-gold border-b border-white/5">
              {t.nav.about}
            </Link>
            <div className="px-6 py-4 bg-gfa-darkGray/30 rounded-lg">
              <div className="text-[10px] text-gfa-gold font-black uppercase tracking-widest mb-4 border-l-2 border-gfa-gold pl-3">{t.nav.certification}</div>
              <div className="grid grid-cols-1 gap-3">
                {certGovLinks.map(gl => (
                  <Link key={gl.path} to={gl.path} className="text-[10px] font-bold uppercase tracking-widest text-gfa-gray hover:text-white">
                    {gl.label}
                  </Link>
                ))}
              </div>
            </div>
            {links.slice(1).map(l => (
              <Link key={l.path} to={l.path} className="block px-6 py-4 text-sm font-bold uppercase tracking-widest text-gfa-gray hover:text-gfa-gold border-b border-white/5">
                {l.label}
              </Link>
            ))}
            <Link to="/contact" className="block px-6 py-6 text-center mt-8 bg-gfa-gold text-gfa-black font-black uppercase text-xs tracking-[0.3em]">
              {t.nav.contact}
            </Link>
          </div>
        </div>
      )}
    </nav>
  );
};

const Footer = () => {
  const { t } = useLocale();
  const currentYear = new Date().getFullYear().toString();
  return (
    <footer className="bg-gfa-black border-t border-gfa-gold/10 pt-20 pb-10">
      <div className="max-w-7xl mx-auto px-4 grid grid-cols-1 md:grid-cols-4 gap-12 mb-16">
        <div className="md:col-span-2">
          <Link to="/" className="flex items-center gap-4 mb-6 group">
            <img 
              src="https://i.ibb.co/B582n2Dk/1755827874220993959.png" 
              alt="GFA Logo" 
              className="h-14 w-auto object-contain transition-transform group-hover:scale-105"
            />
            <div className="flex flex-col">
              <span className="text-2xl font-black gold-gradient leading-none tracking-tighter">GFA</span>
              <span className="text-[7px] tracking-[0.6em] text-gfa-gray uppercase font-bold">Global Film Alliance</span>
            </div>
          </Link>
          <p className="text-gfa-gray text-xs leading-relaxed max-w-sm uppercase tracking-wider font-medium opacity-70">
            Institutional authority for global film certification, talent protection, and creative foundation support.
          </p>
        </div>
        <div>
          <h4 className="text-white text-[10px] font-black uppercase tracking-widest mb-6 border-l-2 border-gfa-gold pl-3">Governance</h4>
          <div className="space-y-4 text-[10px] font-bold uppercase tracking-widest text-gfa-gray">
            <Link to="/governance" className="block hover:text-gfa-gold transition-colors">{t.nav.certGov.governance}</Link>
            <Link to="/standards" className="block hover:text-gfa-gold transition-colors">{t.nav.certGov.standards}</Link>
            <Link to="/transparency" className="block hover:text-gfa-gold transition-colors">{t.nav.certGov.transparency}</Link>
            <Link to="/protection" className="block hover:text-gfa-gold transition-colors">{t.nav.certGov.protection}</Link>
          </div>
        </div>
        <div>
          <h4 className="text-white text-[10px] font-black uppercase tracking-widest mb-6 border-l-2 border-gfa-gold pl-3">Registry</h4>
          <div className="space-y-4 text-[10px] font-bold uppercase tracking-widest text-gfa-gray">
            <Link to="/verify" className="block hover:text-gfa-gold transition-colors">{t.footer.verification}</Link>
            <Link to="/privacy" className="block hover:text-gfa-gold transition-colors">{t.footer.privacy}</Link>
            <Link to="/terms" className="block hover:text-gfa-gold transition-colors">{t.footer.terms}</Link>
          </div>
        </div>
      </div>
      <div className="max-w-7xl mx-auto px-4 pt-10 border-t border-white/5 flex flex-col md:flex-row justify-between items-center gap-6">
        <span className="text-[10px] font-bold uppercase tracking-widest text-gfa-gray/50">
          {t.footer.copyright.replace('{year}', currentYear)}
        </span>
      </div>
    </footer>
  );
};

const App = () => {
  const [locale, setLocale] = useState<Locale>(Locale.EN);
  const t = DICTIONARIES[locale];

  return (
    <LanguageContext.Provider value={{ locale, setLocale, t }}>
      <HashRouter>
        <div className="min-h-screen bg-gfa-black text-white selection:bg-gfa-gold selection:text-gfa-black">
          <Navbar />
          <main className="pt-20">
            <Routes>
              <Route path="/" element={<Home />} />
              <Route path="/about" element={<About />} />
              <Route path="/certification" element={<CertificationPage />} />
              <Route path="/verify" element={<Verify />} />
              <Route path="/support" element={<Support />} />
              <Route path="/casting" element={<Casting />} />
              <Route path="/membership" element={<Membership />} />
              <Route path="/partners" element={<Partners />} />
              <Route path="/events" element={<Events />} />
              <Route path="/contact" element={<Contact />} />
              <Route path="/governance" element={<Governance />} />
              <Route path="/standards" element={<Standards />} />
              <Route path="/transparency" element={<Transparency />} />
              <Route path="/protection" element={<Protection />} />
              <Route path="/directory" element={<Directory />} />
              <Route path="/reporting" element={<Reporting />} />
              <Route path="/privacy" element={<PolicyPage type="privacy" />} />
              <Route path="/terms" element={<PolicyPage type="terms" />} />
            </Routes>
          </main>
          <Footer />
        </div>
      </HashRouter>
    </LanguageContext.Provider>
  );
};

export default App;
