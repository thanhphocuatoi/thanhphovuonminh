
import { useState } from "react";
import {
  ArrowDown,
  ArrowRight,
  ArrowUpRight,
  Building2,
  Clock3,
  Compass,
  Scan,
  X,
  MapPin,
  Menu,
} from "lucide-react";

const videoUrl =
  "https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07IWA1P/hf_20260314_131748_f2ca2a28-fed7-44c8-b9a9-bd9acdd5ec31.mp4";

const chapters = [
  {
    number: "01",
    era: "QUÁ KHỨ",
    title: "Nơi câu chuyện bắt đầu",
    description:
      "Khám phá những dấu ấn lịch sử, những công trình và ký ức đã góp phần tạo nên diện mạo thành phố.",
    image:
      "https://images.unsplash.com/photo-1533929736458-ca588d08c8be?auto=format&fit=crop&w=1400&q=85",
  },
  {
    number: "02",
    era: "HIỆN TẠI",
    title: "Thành phố không ngừng chuyển động",
    description:
      "Từ những con đường quen thuộc đến các công trình kiến trúc mới, thành phố phản chiếu nhịp sống và khát vọng của một Việt Nam năng động.",
    image:
      "https://images.unsplash.com/photo-1583417319070-4a69db38a482?auto=format&fit=crop&w=1400&q=85",
  },
  {
    number: "03",
    era: "TƯƠNG LAI",
    title: "Kiến tạo những chân trời mới",
    description:
      "Thông qua mô hình sa bàn và công nghệ thực tế tăng cường AR, hình dung một đô thị hiện đại, kết nối và bền vững.",
    image:
      "https://images.unsplash.com/photo-1519501025264-65ba15a82390?auto=format&fit=crop&w=1400&q=85",
  },
];

const landmarks = [
  {
    number: "01",
    name: "Cầu Ba Son",
    category: "KẾT NỐI · KIẾN TRÚC",
    description:
      "Một dấu ấn kiến trúc hiện đại trên sông Sài Gòn, kết nối những khu vực đô thị và mở ra góc nhìn mới về thành phố.",
    image:
      "https://images.unsplash.com/photo-1519501025264-65ba15a82390?auto=format&fit=crop&w=1200&q=85",
  },
  {
    number: "02",
    name: "Dấu ấn đô thị",
    category: "DI SẢN · LỊCH SỬ",
    description:
      "Nhìn lại những công trình, không gian và lớp ký ức tạo nên bản sắc riêng của thành phố qua thời gian.",
    image:
      "https://images.unsplash.com/photo-1533929736458-ca588d08c8be?auto=format&fit=crop&w=1200&q=85",
  },
  {
    number: "03",
    name: "Đô thị tương lai",
    category: "ĐỔI MỚI · CÔNG NGHỆ",
    description:
      "Hình dung diện mạo đô thị mai sau qua những ý tưởng quy hoạch, công nghệ và không gian sống mới.",
    image:
      "https://images.unsplash.com/photo-1480714378408-67cf0d13bc1b?auto=format&fit=crop&w=1200&q=85",
  },
];

const styles = `
@import url('https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Inter:wght@400;500;600;700&display=swap');

:root {
  --bg: #071820;
  --bg2: #0c2732;
  --paper: #f1eee7;
  --muted: #aebbc0;
  --accent: #b7d8d8;
  --line: rgba(255,255,255,.15);
}

* { box-sizing: border-box; }

html {
  scroll-behavior: smooth;
  scroll-padding-top: 70px;
}

body {
  margin: 0;
  background: var(--bg);
  color: var(--paper);
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
}

a { color: inherit; text-decoration: none; }
button { font: inherit; cursor: pointer; }
button, a { -webkit-tap-highlight-color: transparent; }
button:focus-visible, a:focus-visible { outline: 2px solid var(--accent); outline-offset: 4px; }
::selection { background: var(--accent); color: var(--bg); }

.site { overflow: hidden; background: var(--bg); }
.container { width: min(1280px, calc(100% - 64px)); margin: auto; }

.nav {
  position: absolute; z-index: 20; top: 0; left: 0; right: 0;
  display: flex; align-items: center; justify-content: space-between;
  padding: 24px max(32px, calc((100vw - 1280px)/2));
  border-bottom: 1px solid rgba(255,255,255,.07);
}

.brand { display: flex; flex-direction: column; gap: 4px; }
.brand-name {
  font-family: 'Instrument Serif', serif; font-size: 30px;
  letter-spacing: .05em; line-height: 1;
}
.brand-name span { color: var(--accent); }
.brand-caption { font-size: 8px; letter-spacing: .32em; color: #c4cccf; }

.nav-links { display: flex; align-items: center; gap: 32px; }
.nav-links a {
  font-size: 11px; color: rgba(255,255,255,.75);
  transition: color .25s;
}
.nav-links a:hover { color: white; }

.outline-button {
  display: inline-flex; justify-content: center; align-items: center; gap: 12px;
  min-height: 46px; padding: 0 22px; border-radius: 999px;
  border: 1px solid rgba(255,255,255,.4);
  color: white; background: rgba(255,255,255,.04);
  font-size: 12px; transition: background .25s, border .25s;
}
.outline-button:hover { background: rgba(255,255,255,.13); border-color: white; }

.menu-button { display: none; }

.hero {
  min-height: 760px; height: 100svh; max-height: 1100px;
  position: relative; display: flex; align-items: center;
  isolation: isolate; background: #0a1c25;
}
.hero-video, .hero-image, .hero-shade {
  position: absolute; inset: 0; width: 100%; height: 100%;
}
.hero-video, .hero-image { object-fit: cover; z-index: -3; }
.hero-shade {
  z-index: -2;
  background:
    linear-gradient(90deg, rgba(3,14,20,.91) 0%, rgba(3,14,20,.68) 39%, rgba(3,14,20,.12) 100%),
    linear-gradient(0deg, rgba(3,14,20,.65), transparent 35%, rgba(3,14,20,.24));
}
.hero-inner { width: min(1280px, calc(100% - 64px)); margin: auto; padding-top: 75px; }
.eyebrow { color: var(--accent); font-size: 10px; letter-spacing: .24em; line-height: 1.8; }
.hero-eyebrow { display: flex; align-items: center; gap: 13px; margin-bottom: 25px; }
.hero-eyebrow:before { content: ''; width: 35px; height: 1px; background: var(--accent); }

h1, h2, h3, p { margin-top: 0; }
h1, h2, h3 { font-family: 'Instrument Serif', serif; font-weight: 400; }
.hero h1 {
  max-width: 920px; margin-bottom: 25px;
  font-size: clamp(58px, 8vw, 112px); line-height: .93;
  letter-spacing: -.035em;
}
.hero h1 em { color: var(--accent); font-weight: 400; }
.hero-description {
  max-width: 560px; color: rgba(255,255,255,.8);
  font-size: 14px; line-height: 1.95;
}
.hero-actions { display: flex; align-items: center; flex-wrap: wrap; gap: 24px; margin-top: 32px; }
.text-link { display: inline-flex; align-items: center; gap: 9px; font-size: 12px; color: rgba(255,255,255,.8); }
.text-link:hover { color: var(--accent); }
.hero-bottom {
  position: absolute; bottom: 27px; left: max(32px, calc((100vw - 1280px)/2));
  right: max(32px, calc((100vw - 1280px)/2));
  display: flex; justify-content: space-between; align-items: end;
  color: rgba(255,255,255,.65); font-size: 9px; letter-spacing: .16em;
}
.hero-location { display: flex; align-items: center; gap: 8px; }
.scroll-cue { display: flex; align-items: center; gap: 10px; }
.scroll-cue span { width: 35px; height: 1px; background: rgba(255,255,255,.5); }

.section { padding: 110px 0; }
.intro-grid { display: grid; grid-template-columns: .9fr 1.1fr; gap: 80px; align-items: end; }
.section-title { font-size: clamp(42px, 5.4vw, 72px); line-height: 1.02; letter-spacing: -.025em; margin: 22px 0 0; }
.section-title em { color: var(--accent); }
.intro-copy { max-width: 620px; justify-self: end; color: var(--muted); font-size: 15px; line-height: 2; margin-bottom: 3px; }

.journey { padding-top: 40px; }
.section-head { display: flex; align-items: end; justify-content: space-between; gap: 30px; margin-bottom: 36px; }
.section-head .section-title { max-width: 750px; }
.section-note { max-width: 300px; color: var(--muted); font-size: 12px; line-height: 1.9; }

.era-tabs { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 18px; }
.era-tab {
  border: 1px solid var(--line); border-radius: 999px; padding: 12px 19px;
  background: transparent; color: #c4cccf; font-size: 10px; letter-spacing: .12em;
  transition: all .25s;
}
.era-tab:hover { border-color: rgba(255,255,255,.5); color: white; }
.era-tab.active { color: var(--bg); background: var(--accent); border-color: var(--accent); }

.chapter-feature {
  display: grid; grid-template-columns: 1.1fr .9fr;
  border: 1px solid var(--line); background: #0a202a;
  animation: reveal .45s ease both;
}
.chapter-image { position: relative; min-height: 440px; overflow: hidden; }
.chapter-image img { width: 100%; height: 100%; position: absolute; inset: 0; object-fit: cover; }
.chapter-number {
  position: absolute; left: 24px; top: 24px; padding: 9px 12px;
  border: 1px solid rgba(255,255,255,.4); font-size: 9px; letter-spacing: .17em;
  background: rgba(5,15,20,.2); backdrop-filter: blur(10px);
}
.chapter-copy { display: flex; flex-direction: column; justify-content: center; padding: clamp(28px, 5vw, 68px); }
.chapter-copy h3 { font-size: clamp(38px, 4vw, 58px); line-height: 1.04; margin: 20px 0; }
.chapter-copy p { color: var(--muted); font-size: 13px; line-height: 2; margin-bottom: 25px; }
.chapter-meta { display: flex; align-items: center; gap: 10px; color: var(--accent); font-size: 9px; letter-spacing: .15em; }

.landmarks-section { background: var(--bg2); }
.landmark-heading { display: flex; justify-content: space-between; align-items: end; gap: 30px; margin-bottom: 45px; }
.landmark-grid { display: grid; grid-template-columns: repeat(3, minmax(0,1fr)); gap: 22px; }
.landmark-card { min-width: 0; }
.landmark-photo { aspect-ratio: 4 / 4.7; position: relative; overflow: hidden; background: #142e38; }
.landmark-photo img { width: 100%; height: 100%; object-fit: cover; filter: saturate(.78); transition: transform .7s, filter .7s; }
.landmark-card:hover .landmark-photo img { transform: scale(1.035); filter: saturate(1); }
.landmark-number { position: absolute; top: 17px; left: 17px; font-size: 11px; letter-spacing: .16em; text-shadow: 0 1px 8px #000; }
.landmark-category {
  position: absolute; bottom: 15px; left: 15px; padding: 9px 11px;
  background: rgba(5,15,20,.55); border: 1px solid rgba(255,255,255,.3);
  font-size: 8px; letter-spacing: .12em; backdrop-filter: blur(8px);
}
.landmark-card h3 { font-size: 32px; margin: 21px 0 10px; }
.landmark-card p { color: var(--muted); font-size: 12px; line-height: 1.9; }
.disclaimer { margin-top: 24px; color: rgba(255,255,255,.4); font-size: 10px; line-height: 1.8; }

.ar-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 75px; align-items: center; }
.ar-copy .section-title { font-size: clamp(50px, 6vw, 78px); }
.ar-description { margin: 26px 0 32px; max-width: 570px; color: var(--muted); font-size: 13px; line-height: 2; }
.steps { border-top: 1px solid var(--line); }
.step { display: flex; gap: 18px; padding: 19px 0; border-bottom: 1px solid var(--line); }
.step-number { color: var(--accent); font-size: 10px; padding-top: 4px; }
.step h4 { margin: 0 0 7px; font-size: 12px; font-weight: 500; }
.step p { margin: 0; color: var(--muted); font-size: 11px; line-height: 1.8; }

.ar-visual { position: relative; min-height: 560px; overflow: hidden; background: #102d3a; }
.ar-visual img { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: cover; }
.ar-visual:after { content: ''; position: absolute; inset: 0; background: linear-gradient(0deg, rgba(3,14,20,.8), rgba(3,14,20,.15)); }
.ar-label { position: absolute; z-index: 1; top: 20px; left: 20px; font-size: 9px; letter-spacing: .2em; }
.ar-center { position: absolute; z-index: 2; inset: 0; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; padding: 25px; }
.scan-icon { width: 75px; height: 75px; display: grid; place-items: center; border: 1px solid rgba(255,255,255,.55); border-radius: 50%; background: rgba(5,20,28,.25); backdrop-filter: blur(8px); }
.ar-center .overline { font-size: 9px; letter-spacing: .24em; margin: 23px 0 13px; }
.ar-center h3 { font-size: clamp(35px, 4vw, 52px); line-height: 1.05; margin: 0; }
.ar-center p { max-width: 290px; color: rgba(255,255,255,.8); font-size: 11px; line-height: 1.8; margin-top: 15px; }
.ar-bottom { position: absolute; z-index: 2; right: 20px; bottom: 20px; font-size: 9px; letter-spacing: .17em; }

.final-cta { border-top: 1px solid var(--line); padding: 70px 0 100px; }
.final-row { display: flex; align-items: end; justify-content: space-between; gap: 30px; }
.final-row h2 { font-size: clamp(45px, 6.4vw, 82px); line-height: .98; margin: 20px 0 0; }

.footer { border-top: 1px solid var(--line); padding: 25px 0; }
.footer-row { display: flex; justify-content: space-between; align-items: center; gap: 20px; color: rgba(255,255,255,.45); font-size: 9px; letter-spacing: .1em; }

.mobile-menu {
  display: none; position: absolute; top: 82px; left: 20px; right: 20px;
  padding: 22px; background: rgba(5,22,30,.96); border: 1px solid var(--line);
  backdrop-filter: blur(20px);
}
.mobile-menu a { padding: 13px 0; display: block; color: #d7dfe1; font-size: 13px; border-bottom: 1px solid rgba(255,255,255,.08); }
.mobile-menu a:last-child { border-bottom: 0; }

@keyframes reveal {
  from { opacity: 0; transform: translateY(12px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (min-width: 1500px) {
  .hero h1 { font-size: 116px; }
}
@media (max-width: 900px) {
  .nav { padding: 20px 28px; }
  .nav-links { gap: 17px; }
  .nav-links a { font-size: 10px; }
  .brand-caption { letter-spacing: .18em; }
  .intro-grid { gap: 35px; }
  .ar-grid { gap: 35px; }
  .chapter-image { min-height: 380px; }
  .ar-visual { min-height: 470px; }
}
@media (max-width: 700px) {
  .container, .hero-inner { width: calc(100% - 40px); }
  .nav { padding: 18px 20px; }
  .brand-name { font-size: 25px; }
  .brand-caption { font-size: 7px; }
  .nav-links, .nav-cta { display: none; }
  .menu-button { display: inline-flex; align-items: center; justify-content: center; width: 42px; height: 42px; border: 1px solid var(--line); border-radius: 50%; color: white; background: rgba(255,255,255,.05); }
  .mobile-menu { display: block; }
  .hero { min-height: 740px; height: 100svh; }
  .hero-shade { background: linear-gradient(90deg, rgba(3,14,20,.8), rgba(3,14,20,.36)), linear-gradient(0deg, rgba(3,14,20,.65), transparent 70%); }
  .hero h1 { font-size: clamp(52px, 13vw, 78px); }
  .hero-description { font-size: 12px; }
  .hero-bottom { left: 20px; right: 20px; bottom: 20px; font-size: 8px; }
  .scroll-cue { display: none; }
  .section { padding: 75px 0; }
  .intro-grid, .chapter-feature, .ar-grid { grid-template-columns: 1fr; }
  .intro-grid { gap: 25px; }
  .intro-copy { justify-self: start; font-size: 13px; }
  .section-head, .landmark-heading { align-items: start; flex-direction: column; }
  .section-note { max-width: 100%; }
  .chapter-image { min-height: 300px; }
  .chapter-copy { padding: 28px 24px 32px; }
  .landmark-grid { grid-template-columns: 1fr; gap: 38px; }
  .landmark-photo { aspect-ratio: 5 / 4; }
  .landmark-card h3 { font-size: 30px; }
  .ar-visual { min-height: 420px; }
  .final-row { align-items: start; flex-direction: column; }
  .final-cta { padding: 55px 0 70px; }
  .footer-row { align-items: start; flex-direction: column; line-height: 1.8; }
}
@media (prefers-reduced-motion: reduce) {
  *, *:before, *:after { scroll-behavior: auto !important; animation-duration: .01ms !important; transition-duration: .01ms !important; }
}
`;

export default function App() {
  const [menuOpen, setMenuOpen] = useState(false);
  const [activeEra, setActiveEra] = useState("HIỆN TẠI");

  const closeMenu = () => setMenuOpen(false);

  return (
    <div className="site">
      <style>{styles}</style>

      <header className="nav">
        <a href="#home" className="brand" onClick={closeMenu}>
          <span className="brand-name">VIỆT NAM<span>✦</span></span>
          <span className="brand-caption">SA BÀN & CÔNG NGHỆ AR</span>
        </a>

        <div className="nav-links">
          <a href="#home">Trang chủ</a>
          <a href="#journey">Hành trình</a>
          <a href="#landmarks">Công trình</a>
          <a href="#ar">Trải nghiệm AR</a>
          <a href="#about">Về dự án</a>
        </div>

        <a href="#journey" className="outline-button nav-cta">
          Khám phá ngay <ArrowRight size={15} />
        </a>

        <button
          className="menu-button"
          onClick={() => setMenuOpen(!menuOpen)}
          aria-label={menuOpen ? "Đóng menu" : "Mở menu"}
          aria-expanded={menuOpen}
        >
          {menuOpen ? <X size={20} /> : <Menu size={20} />}
        </button>

        {menuOpen && (
          <div className="mobile-menu">
            {[
              ["Trang chủ", "#home"],
              ["Hành trình", "#journey"],
              ["Công trình", "#landmarks"],
              ["Trải nghiệm AR", "#ar"],
              ["Về dự án", "#about"],
            ].map(([label, href]) => (
              <a key={href} href={href} onClick={closeMenu}>{label}</a>
            ))}
          </div>
        )}
      </header>

      <main>
        <section className="hero" id="home">
          <video
            className="hero-video"
            src={videoUrl}
            autoPlay
            muted
            loop
            playsInline
            preload="metadata"
            aria-hidden="true"
          />
          <div className="hero-shade" />

          <div className="hero-inner">
            <div className="eyebrow hero-eyebrow">MỘT HÀNH TRÌNH · BA THỜI ĐẠI</div>
            <h1>
              Thành phố qua
              <br />
              <em>những lát cắt</em>
              <br />
              thời gian.
            </h1>
            <p className="hero-description">
              Từ những dấu ấn của quá khứ đến nhịp sống hiện đại và diện
              mạo tương lai. Cùng khám phá hành trình chuyển mình của
              thành phố qua sa bàn kết hợp công nghệ thực tế tăng cường AR.
            </p>
            <div className="hero-actions">
              <a className="outline-button" href="#journey">
                Bắt đầu hành trình <ArrowRight size={16} />
              </a>
              <a className="text-link" href="#ar">
                Khám phá công nghệ AR <ArrowUpRight size={15} />
              </a>
            </div>
          </div>

          <div className="hero-bottom">
            <div className="hero-location"><MapPin size={14} /> TP. HỒ CHÍ MINH</div>
            <a href="#journey" className="scroll-cue">
              <span /> CUỘN ĐỂ KHÁM PHÁ <ArrowDown size={14} />
            </a>
            <div>DI SẢN · ĐÔ THỊ · CÔNG NGHỆ</div>
          </div>
        </section>

        <section className="section" id="about">
          <div className="container intro-grid">
            <div>
              <div className="eyebrow">01 / TẦM NHÌN DỰ ÁN</div>
              <h2 className="section-title">
                Một thành phố.
                <br />
                <em>Nhiều câu chuyện.</em>
              </h2>
            </div>
            <p className="intro-copy">
              Mỗi công trình là một dấu mốc. Mỗi giai đoạn là một câu
              chuyện về con người, văn hóa và sự phát triển. Dự án đưa
              những lát cắt ấy vào một không gian khám phá trực quan,
              nơi lịch sử không chỉ được nhìn thấy mà còn có thể được
              tương tác thông qua sa bàn và công nghệ AR.
            </p>
          </div>
        </section>

        <section className="section journey" id="journey">
          <div className="container">
            <div className="section-head">
              <div>
                <div className="eyebrow">02 / DÒNG CHẢY THỜI GIAN</div>
                <h2 className="section-title">Ba lát cắt, một hành trình</h2>
              </div>
              <p className="section-note">
                Chạm vào từng giai đoạn để khám phá cách thành phố thay đổi
                qua thời gian.
              </p>
            </div>

            <div className="era-tabs" role="tablist" aria-label="Các giai đoạn">
              {chapters.map((chapter) => (
                <button
                  key={chapter.era}
                  role="tab"
                  aria-selected={activeEra === chapter.era}
                  className={`era-tab ${activeEra === chapter.era ? "active" : ""}`}
                  onClick={() => setActiveEra(chapter.era)}
                >
                  {chapter.era}
                </button>
              ))}
            </div>

            {chapters.filter((chapter) => chapter.era === activeEra).map((chapter) => (
              <article className="chapter-feature" key={chapter.era}>
                <div className="chapter-image">
                  <img src={chapter.image} alt={`Minh họa ${chapter.era.toLowerCase()}`} loading="lazy" />
                  <span className="chapter-number">CHƯƠNG {chapter.number}</span>
                </div>
                <div className="chapter-copy">
                  <div className="eyebrow">{chapter.era}</div>
                  <h3>{chapter.title}</h3>
                  <p>{chapter.description}</p>
                  <div className="chapter-meta"><Clock3 size={15} /> DÒNG CHẢY ĐÔ THỊ</div>
                </div>
              </article>
            ))}
          </div>
        </section>

        <section className="section landmarks-section" id="landmarks">
          <div className="container">
            <div className="landmark-heading">
              <div>
                <div className="eyebrow">03 / NHỮNG DẤU ẤN KIẾN TRÚC</div>
                <h2 className="section-title">
                  Thành phố được kể
                  <br />
                  qua những công trình.
                </h2>
              </div>
              <Building2 size={42} strokeWidth={1} color="#b7d8d8" />
            </div>

            <div className="landmark-grid">
              {landmarks.map((item) => (
                <article className="landmark-card" key={item.number}>
                  <div className="landmark-photo">
                    <img src={item.image} alt={item.name} loading="lazy" />
                    <span className="landmark-number">{item.number}</span>
                    <span className="landmark-category">{item.category}</span>
                  </div>
                  <h3>{item.name}</h3>
                  <p>{item.description}</p>
                </article>
              ))}
            </div>
            <p className="disclaimer">
              Lưu ý: Hình ảnh hiện là ảnh minh họa. Hãy thay bằng ảnh tư liệu
              xác thực của từng công trình để bảo đảm tính chính xác cho dự án.
            </p>
          </div>
        </section>

        <section className="section" id="ar">
          <div className="container ar-grid">
            <div className="ar-copy">
              <div className="eyebrow">04 / TRẢI NGHIỆM TƯƠNG TÁC</div>
              <h2 className="section-title">
                Không chỉ nhìn.
                <br />
                <em>Hãy khám phá.</em>
              </h2>
              <p className="ar-description">
                Sa bàn vật lý kết hợp công nghệ thực tế tăng cường AR mở ra
                cách tiếp cận mới với lịch sử và kiến trúc. Khi hệ thống AR
                được tích hợp, người xem có thể quét các điểm đánh dấu để
                khám phá hình ảnh, nội dung số và mô hình 3D tương ứng.
              </p>

              <div className="steps">
                {[
                  ["01", "Quan sát sa bàn", "Khám phá tổng thể không gian đô thị."],
                  ["02", "Quét điểm đánh dấu", "Kích hoạt nội dung AR bằng thiết bị tương thích."],
                  ["03", "Khám phá các lớp thông tin", "Tìm hiểu công trình và sự thay đổi theo thời gian."],
                ].map(([number, title, description]) => (
                  <div className="step" key={number}>
                    <span className="step-number">{number}</span>
                    <div>
                      <h4>{title}</h4>
                      <p>{description}</p>
                    </div>
                  </div>
                ))}
              </div>
            </div>

            <div className="ar-visual">
              <img
                src="https://images.unsplash.com/photo-1486406146926-c627a92ad1ab?auto=format&fit=crop&w=1400&q=85"
                alt="Minh họa kiến trúc đô thị"
                loading="lazy"
              />
              <span className="ar-label">AR EXPERIENCE / 01</span>
              <div className="ar-center">
                <div className="scan-icon"><Scan size={34} strokeWidth={1.2} /></div>
                <p className="overline">AUGMENTED REALITY</p>
                <h3>Thành phố trong tầm mắt</h3>
                <p>Từ sa bàn thực tế đến thế giới số tương tác.</p>
              </div>
              <span className="ar-bottom">EXPLORE THE CITY</span>
            </div>
          </div>
        </section>

        <section className="final-cta">
          <div className="container final-row">
            <div>
              <div className="eyebrow">MỘT CÁCH NHÌN MỚI VỀ THÀNH PHỐ</div>
              <h2>
                Quá khứ tạo nền.
                <br />
                Tương lai tiếp nối.
              </h2>
            </div>
            <a href="#home" className="outline-button">
              Trở về đầu trang <ArrowUpRight size={16} />
            </a>
          </div>
        </section>
      </main>

      <footer className="footer">
        <div className="container footer-row">
          <span>© {new Date().getFullYear()} VIỆT NAM QUA NHỮNG LÁT CẮT THỜI GIAN</span>
          <span>DI SẢN · KIẾN TRÚC · CÔNG NGHỆ AR</span>
        </div>
      </footer>
    </div>
  );
}


