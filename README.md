# audrey.shitamoto
/* ============================================================
   Home.tsx — AFS Portfolio v3
   Design: Modern dark editorial — clean DM Sans, pure black,
   white as structural light element, particle data visualization.
   Flow: Intro → Hero → Hall of Fame (horizontal) → Portfolio Archive → Skills → Contact
   ============================================================ */

import { useEffect, useRef, useState, useCallback } from 'react';
import ParticleField from '@/components/ParticleField';
import AFSCursor from '@/components/AFSCursor';
import PhotoCarousel from '@/components/PhotoCarousel';
import ArchivesBackground from '@/components/ArchivesBackground';

// ─── Data ────────────────────────────────────────────────────

const HALL_OF_FAME = [
  {
    index: '01',
    title: 'Gilead Sciences\nAI Compliance Audit',
    tag: 'Healthcare / AI',
    desc: 'Comprehensive audit framework for AI-driven clinical trial data systems. Identified 23 compliance gaps, reduced regulatory risk exposure by 40%.',
    stats: [
      { value: '23', label: 'Gaps Identified' },
      { value: '40%', label: 'Risk Reduction' },
      { value: '6mo', label: 'Engagement' },
    ],
    bg: 'https://d2xsxph8kpxj0f.cloudfront.net/310519663529869006/YkJEd8CNVZC58RGSkHCa2q/afs-v3-particle-burst-9v396jUkw4rSrw5ZQ7n5GB.webp',
  },
  {
    index: '02',
    title: 'NYU Fencing\nMedia Strategy',
    tag: 'Sports / Media',
    desc: 'Rebuilt digital presence for NYU Fencing program. Grew social following 280%, secured two brand partnerships, produced 40+ video assets.',
    stats: [
      { value: '280%', label: 'Audience Growth' },
      { value: '2', label: 'Partnerships' },
      { value: '40+', label: 'Video Assets' },
    ],
    bg: 'https://d2xsxph8kpxj0f.cloudfront.net/310519663529869006/YkJEd8CNVZC58RGSkHCa2q/afs-v3-geometric-flow-FY9rbXSPuzfZf5emLjYTyW.webp',
  },
  {
    index: '03',
    title: 'Boulder Fintech\nData Dashboard',
    tag: 'Finance / Data',
    desc: 'End-to-end analytics platform for a Series A fintech startup. Real-time portfolio tracking, risk modeling, and executive reporting suite.',
    stats: [
      { value: '$2.4M', label: 'AUM Tracked' },
      { value: '99.9%', label: 'Uptime' },
      { value: '12', label: 'Dashboards' },
    ],
    bg: 'https://d2xsxph8kpxj0f.cloudfront.net/310519663529869006/YkJEd8CNVZC58RGSkHCa2q/afs-v3-data-portrait-DW6gbWqsxXzaJmAsQKP7ou.webp',
  },
];

const PORTFOLIO_ITEMS = [
  {
    num: '001',
    title: 'Brand Identity System',
    category: 'Creative Direction',
    desc: 'Visual identity development for a luxury wellness brand. Typography, color system, photography direction, and brand guidelines.',
  },
  {
    num: '002',
    title: 'Market Entry Analysis',
    category: 'Strategy / Research',
    desc: 'Competitive landscape analysis and go-to-market strategy for APAC expansion. Primary research across 6 markets.',
  },
  {
    num: '003',
    title: 'Editorial Photography',
    category: 'Visual Media',
    desc: 'Art direction and photography for a quarterly print publication. 3 issues, 200+ images, full creative oversight.',
  },
  {
    num: '004',
    title: 'UX Research Study',
    category: 'Research / Design',
    desc: 'Mixed-methods user research for a B2B SaaS platform. 40 interviews, usability testing, synthesis, and design recommendations.',
  },
  {
    num: '005',
    title: 'Executive Communications',
    category: 'Strategy / Writing',
    desc: 'Speechwriting and communications strategy for C-suite executives at a Fortune 500. Keynote addresses, investor materials, media prep.',
  },
  {
    num: '006',
    title: 'Data Visualization Suite',
    category: 'Data / Design',
    desc: 'Custom data visualization library for a climate tech company. Interactive charts, annual report graphics, investor deck assets.',
  },
];

const EDUCATION = [
  {
    year: '2022 — Present',
    institution: 'New York University',
    degree: 'B.S. Business & Political Economy',
    location: 'New York, NY',
    desc: 'Concentrating in data analytics, strategic communications, and global markets. Active member of the Fencing team and Media & Communications board.',
    tags: ['Data Analytics', 'Strategic Comms', 'Global Markets', 'Varsity Fencing'],
  },
  {
    year: '2021 — 2022',
    institution: 'University of Colorado Boulder',
    degree: 'Business Administration',
    location: 'Boulder, CO',
    desc: 'Coursework in finance, entrepreneurship, and organizational behavior. Participated in the Leeds School of Business case competition program.',
    tags: ['Finance', 'Entrepreneurship', 'Case Competitions'],
  },
  {
    year: '2018 — 2022',
    institution: 'High School',
    degree: 'Diploma',
    location: 'United States',
    desc: 'Advanced coursework in mathematics, economics, and visual arts. Competed nationally in fencing.',
    tags: ['Economics', 'Visual Arts', 'National Fencing'],
  },
];

const CERTIFICATIONS = [
  {
    issuer: 'Google',
    title: 'Data Analytics Professional Certificate',
    date: '2023',
    link: '#',
  },
  {
    issuer: 'Coursera / IBM',
    title: 'Data Science Fundamentals with Python and SQL',
    date: '2023',
    link: '#',
  },
  {
    issuer: 'Tableau',
    title: 'Tableau Desktop Specialist',
    date: '2023',
    link: '#',
  },
  {
    issuer: 'HubSpot Academy',
    title: 'Content Marketing Certification',
    date: '2022',
    link: '#',
  },
  {
    issuer: 'Meta Blueprint',
    title: 'Digital Marketing Associate',
    date: '2022',
    link: '#',
  },
  {
    issuer: 'LinkedIn Learning',
    title: 'Strategic Communications & Executive Presence',
    date: '2024',
    link: '#',
  },
];

const SKILLS = [
  {
    label: 'Data & Analytics',
    tags: ['Python', 'SQL', 'Tableau', 'Power BI', 'R', 'Excel', 'Data Modeling', 'Statistical Analysis'],
  },
  {
    label: 'Strategy & Research',
    tags: ['Market Research', 'Competitive Analysis', 'Go-to-Market', 'Brand Strategy', 'UX Research', 'Stakeholder Management'],
  },
  {
    label: 'Creative & Media',
    tags: ['Art Direction', 'Editorial Design', 'Photography', 'Video Production', 'Adobe Creative Suite', 'Figma'],
  },
  {
    label: 'Communications',
    tags: ['Executive Communications', 'Speechwriting', 'Content Strategy', 'Social Media', 'PR', 'Investor Relations'],
  },
];

// ─── Component ───────────────────────────────────────────────

export default function Home() {
  const [introVisible, setIntroVisible] = useState(true);
  const [introExiting, setIntroExiting] = useState(false);
  const [scrollProgress, setScrollProgress] = useState(0);
  const [showPortfolioCTA, setShowPortfolioCTA] = useState(false);
  const portfolioRef = useRef<HTMLDivElement>(null);
  const heroRef = useRef<HTMLDivElement>(null);
  const revealRefs = useRef<HTMLElement[]>([]);

  // ── Intro exit
  const handleEnter = useCallback(() => {
    setIntroExiting(true);
    setTimeout(() => setIntroVisible(false), 1200);
  }, []);

  // Auto-enter after 3.5s
  useEffect(() => {
    const t = setTimeout(handleEnter, 3500);
    return () => clearTimeout(t);
  }, [handleEnter]);

  // ── Scroll tracking
  useEffect(() => {
    const onScroll = () => {
      const scrollY = window.scrollY;
      const maxScroll = document.documentElement.scrollHeight - window.innerHeight;
      setScrollProgress(maxScroll > 0 ? scrollY / maxScroll : 0);
      setShowPortfolioCTA(scrollY > window.innerHeight * 0.5);
    };
    window.addEventListener('scroll', onScroll, { passive: true });
    return () => window.removeEventListener('scroll', onScroll);
  }, []);

  // ── Intersection observer for reveal animations
  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
          }
        });
      },
      { threshold: 0.1, rootMargin: '0px 0px -60px 0px' }
    );

    const elements = document.querySelectorAll('.reveal');
    elements.forEach(el => observer.observe(el));

    return () => observer.disconnect();
  }, [introVisible]);

  const scrollToPortfolio = () => {
    portfolioRef.current?.scrollIntoView({ behavior: 'smooth' });
  };

  const scrollToTop = () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
  };

  return (
    <div style={{ background: '#000000', minHeight: '100vh', position: 'relative' }}>
      {/* Living particle field — always present */}
      <ParticleField />

      {/* Custom cursor */}
      <AFSCursor />

      {/* ── INTRO SCREEN ─────────────────────────────────── */}
      {introVisible && (
        <div className={`intro-section ${introExiting ? 'exiting' : ''}`}>
          {/* Intro particle canvas overlay */}
          <div style={{ position: 'absolute', inset: 0, overflow: 'hidden', pointerEvents: 'none' }}>
            <IntroCanvas />
          </div>

          <div style={{ position: 'relative', zIndex: 2, textAlign: 'center' }}>
            <div className="afs-title-intro">
              <span className="letter">A</span>
              <span className="letter">F</span>
              <span className="letter">S</span>
            </div>
            <div className="intro-subtitle">Audrey Freya Shitamoto</div>
          </div>

          <button className="intro-enter-btn" onClick={handleEnter}>
            Enter
          </button>
        </div>
      )}

      {/* ── NAVIGATION ───────────────────────────────────── */}
      <nav className="afs-nav" style={{ opacity: introVisible ? 0 : 1, transition: 'opacity 0.8s ease 0.3s' }}>
        <a href="#" className="afs-nav-logo" onClick={scrollToTop}>AFS</a>

        <div className="afs-nav-center">
          <button className="afs-nav-return" onClick={scrollToTop}>
            ↑ Return
          </button>
        </div>

        <div className="afs-nav-right">
          <a href="#hall-of-fame" className="afs-nav-link">Work</a>
          <a href="#portfolio" className="afs-nav-link">Archive</a>
          <a href="#contact" className="afs-nav-link">Contact</a>
        </div>
      </nav>

      {/* ── SCROLL PROGRESS ──────────────────────────────── */}
      <div className="scroll-progress" style={{ opacity: introVisible ? 0 : 1, transition: 'opacity 0.8s ease' }}>
        <div className="scroll-progress-label">Scroll</div>
        <div className="scroll-progress-track">
          <div
            className="scroll-progress-fill"
            style={{ height: `${scrollProgress * 100}%` }}
          />
        </div>
      </div>

      {/* ── PORTFOLIO CTA (bottom fixed) ─────────────────── */}
      <a
        href="#portfolio"
        className={`portfolio-cta ${showPortfolioCTA ? 'visible' : ''}`}
        onClick={(e) => { e.preventDefault(); scrollToPortfolio(); }}
      >
        ↓ Portfolio Archive
      </a>

      {/* ── MAIN CONTENT ─────────────────────────────────── */}
      <main
        className="afs-main"
        style={{ opacity: introVisible ? 0 : 1, transition: 'opacity 0.8s ease 0.5s' }}
      >
        {/* ── HERO ─────────────────────────────────────── */}
        <section ref={heroRef} className="hero-section" style={{ minHeight: '100vh' }}>
          <div className="hero-bg-image" />

          {/* Headshot slot — replace src with your actual headshot */}
          <div className="hero-headshot-slot" style={{ top: '8rem', right: '2.5rem' }}>
            <div className="hero-headshot-inner">
              <img
                src="https://d2xsxph8kpxj0f.cloudfront.net/310519663529869006/YkJEd8CNVZC58RGSkHCa2q/afs-headshot_e5f2c500.png"
                alt="AFS"
                className="hero-headshot-img"
                style={{ width: '140px', height: '140px' }}
              />

            </div>
          </div>

          <div className="hero-content">
            <div className="hero-eyebrow reveal">
              Resume + Portfolio
            </div>

            <h1 className="hero-name reveal reveal-delay-1">
              Audrey<br />
              Freya<br />
              Shitamoto
            </h1>

            <div className="hero-descriptor reveal reveal-delay-2">
              <p className="hero-role">
                Digital strategist. Data analyst.<br />
                Creative director. Based in New York.
              </p>
              <p className="hero-role" style={{ marginLeft: '3rem' }}>
                Bridging the space between<br />
                analytical rigor and editorial vision.
              </p>
            </div>
          </div>

          <div className="hero-scroll-indicator">
            <div className="hero-scroll-line" />
            <span className="hero-scroll-label">Scroll</span>
          </div>
        </section>

        {/* ── HALL OF FAME ─────────────────────────────── */}
        <section id="hall-of-fame" className="tunnel-section">
          <div className="tunnel-header">
            <div>
              <div className="text-label reveal" style={{ marginBottom: '1rem' }}>
                — Selected Works
              </div>
              <h2 className="tunnel-title reveal reveal-delay-1">
                Portfolio<br />Highlights
              </h2>
            </div>
            <div className="tunnel-meta reveal reveal-delay-2">
              <div>Featured Projects</div>
              <div>2023 — 2025</div>
            </div>
          </div>

          {/* Photo carousel — right below Portfolio Highlights title */}
          <PhotoCarousel />

          <div className="tunnel-track" id="tunnel-track">
            {HALL_OF_FAME.map((project) => (
              <div key={project.index} className="project-card">
                <div
                  className="project-card-image"
                  style={{ backgroundImage: `url(${project.bg})` }}
                />
                <div className="project-card-content">
                  <div className="project-card-top">
                    <span className="project-card-index">{project.index}</span>
                    <span className="project-card-tag">{project.tag}</span>
                  </div>

                  <div className="project-card-bottom">
                    <h3 className="project-card-title" style={{ whiteSpace: 'pre-line' }}>
                      {project.title}
                    </h3>
                    <p className="project-card-desc">{project.desc}</p>

                    <div className="project-card-stats">
                      {project.stats.map((stat) => (
                        <div key={stat.label} className="stat-item">
                          <span className="stat-value">{stat.value}</span>
                          <span className="stat-label">{stat.label}</span>
                        </div>
                      ))}
                    </div>
                  </div>
                </div>
                <div className="project-card-line" />
              </div>
            ))}
          </div>

        </section>

         {/* ── STUDENT BIO TRANSITION + SECTION ──────────────── */}
        {/* Animated FX lead-in */}
        <div style={{
          position: 'relative',
          height: '30vh',
          background: 'linear-gradient(to bottom, #0a151e 0%, #000 100%)',
          overflow: 'hidden',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}>
          {/* Horizontal scan lines */}
          {[0.2, 0.5, 0.8].map((pos, i) => (
            <div key={i} style={{
              position: 'absolute',
              left: 0,
              right: 0,
              top: `${pos * 100}%`,
              height: '1px',
              background: `rgba(255,255,255,${0.04 + i * 0.02})`,
            }} />
          ))}
          {/* Animated vertical line descending */}
          <div style={{
            position: 'absolute',
            left: '50%',
            top: 0,
            width: '1px',
            height: '100%',
            background: 'linear-gradient(to bottom, transparent, rgba(255,255,255,0.35), transparent)',
            animation: 'scrollPulse 2.5s ease-in-out infinite',
          }} />
          {/* Label */}
          <div style={{
            fontFamily: 'Space Mono, monospace',
            fontSize: '0.55rem',
            letterSpacing: '0.35em',
            textTransform: 'uppercase',
            color: 'rgba(255,255,255,0.25)',
            position: 'relative',
            zIndex: 2,
          }}>Identity · Background · Profile</div>
          {/* Corner brackets */}
          {[['0','0'],['0','auto'],['auto','0'],['auto','auto']].map(([t,b], i) => (
            <div key={i} style={{
              position: 'absolute',
              top: t === '0' ? '1.5rem' : 'auto',
              bottom: b === 'auto' ? '1.5rem' : 'auto',
              left: i < 2 ? '2.5rem' : 'auto',
              right: i >= 2 ? '2.5rem' : 'auto',
              width: '20px',
              height: '20px',
              borderTop: t === '0' ? '1px solid rgba(255,255,255,0.15)' : 'none',
              borderBottom: b === 'auto' ? '1px solid rgba(255,255,255,0.15)' : 'none',
              borderLeft: i < 2 ? '1px solid rgba(255,255,255,0.15)' : 'none',
              borderRight: i >= 2 ? '1px solid rgba(255,255,255,0.15)' : 'none',
            }} />
          ))}
        </div>

        <section className="student-bio-section">
          <div>
            <div className="student-bio-label reveal">— Identity</div>
            <h2 className="student-bio-sidebar-title reveal reveal-delay-1">
              Student<br />Profile
            </h2>
          </div>
          <div className="student-bio-content">
            <div className="student-bio-row reveal">
              <div className="student-bio-row-label">Currently</div>
              <div className="student-bio-row-value">
                <strong>B.S. Business & Political Economy</strong>
                <span className="bio-sub">New York University — Stern / CAS · Expected 2026</span>
              </div>
            </div>
            <div className="student-bio-row reveal reveal-delay-1">
              <div className="student-bio-row-label">Location</div>
              <div className="student-bio-row-value">
                <strong>New York, NY</strong>
                <span className="bio-sub">Based in Manhattan · Open to remote & global</span>
              </div>
            </div>
            <div className="student-bio-row reveal reveal-delay-2">
              <div className="student-bio-row-label">Focus Areas</div>
              <div className="student-bio-row-value">
                Data analytics, strategic communications, creative direction, and the intersection of technology with editorial storytelling.
              </div>
            </div>
            <div className="student-bio-row reveal reveal-delay-3">
              <div className="student-bio-row-label">Previously</div>
              <div className="student-bio-row-value">
                <strong>University of Colorado Boulder</strong>
                <span className="bio-sub">Leeds School of Business · 2021 – 2022</span>
              </div>
            </div>
            <div className="student-bio-row reveal reveal-delay-4">
              <div className="student-bio-row-label">Athletics</div>
              <div className="student-bio-row-value">
                <strong>NYU Varsity Fencing</strong>
                <span className="bio-sub">Competed nationally · Media & Communications board member</span>
              </div>
            </div>
          </div>
        </section>

        {/* ── PORTFOLIO ARCHIVE ──────────────────────── */}
        <section id="portfolio" ref={portfolioRef} className="portfolio-section" style={{ position: 'relative', overflow: 'hidden' }}>
          {/* Animated canvas background */}
          <ArchivesBackground />
          <div className="portfolio-section-header" style={{ position: 'relative', zIndex: 1 }}>
            <h2 className="portfolio-section-title reveal">
              Archives
            </h2>
            <p className="portfolio-section-intro reveal reveal-delay-1">
              A selection of work spanning strategy, data, creative direction,
              and communications. Each project represents a distinct intersection
              of analytical and editorial thinking.
            </p>
          </div>

          <div className="portfolio-grid" style={{ position: 'relative', zIndex: 1 }}>
            {PORTFOLIO_ITEMS.map((item, i) => (
              <div key={item.num} className={`portfolio-item reveal reveal-delay-${(i % 3) + 1}`}>
                <div className="portfolio-item-hover-line" />
                <div className="portfolio-item-number">{item.num}</div>
                <div className="portfolio-item-category">{item.category}</div>
                <h3 className="portfolio-item-title">{item.title}</h3>
                <p className="portfolio-item-desc">{item.desc}</p>
              </div>
            ))}
          </div>
        </section>

        {/* ── EDUCATION ────────────────────────────────── */}
        <section className="education-section">
          <div className="education-header">
            <h2 className="education-title reveal">
              Education
            </h2>
            <div className="education-meta-label reveal reveal-delay-1">
              <div>Academic History</div>
              <div>2018 — Present</div>
            </div>
          </div>
          <div className="education-entries">
            {EDUCATION.map((entry, i) => (
              <div key={entry.institution} className={`education-entry reveal reveal-delay-${i + 1}`}>
                <div>
                  <div className="education-entry-year">{entry.year}</div>
                </div>
                <div>
                  <div className="education-entry-institution">{entry.institution}</div>
                  <div className="education-entry-degree">{entry.degree}</div>
                  <div className="education-entry-location">{entry.location}</div>
                  <div className="education-entry-tags">
                    {entry.tags.map(tag => (
                      <span key={tag} className="education-entry-tag">{tag}</span>
                    ))}
                  </div>
                </div>
                <div className="education-entry-desc">{entry.desc}</div>
              </div>
            ))}
          </div>
        </section>

        {/* ── DATA PORTRAIT DIVIDER ────────────────────── */}
        <div style={{ position: 'relative', height: '70vh', overflow: 'hidden', background: '#0a151e' }}>
          {/* Left: quote text */}
          <div style={{
            position: 'absolute',
            left: '2.5rem',
            top: '50%',
            transform: 'translateY(-50%)',
            zIndex: 2,
            maxWidth: '50%',
          }}>
            <div className="text-label reveal" style={{ marginBottom: '1.5rem' }}>— About</div>
            <p style={{
              fontFamily: 'DM Sans, sans-serif',
              fontSize: 'clamp(1.5rem, 3vw, 2.8rem)',
              fontWeight: 300,
              letterSpacing: '-0.02em',
              lineHeight: 1.3,
              color: 'rgba(255,255,255,1)',
              maxWidth: '520px',
            }} className="reveal reveal-delay-1">
              "I work at the intersection of<br />
              data and narrative — finding the<br />
              story inside the structure."
            </p>
          </div>

          {/* Right: new image — flowing silk/light figure */}
          <div style={{
            position: 'absolute',
            right: 0,
            top: 0,
            bottom: 0,
            width: '48%',
            overflow: 'hidden',
          }}>
            <img
              src="https://d2xsxph8kpxj0f.cloudfront.net/310519663529869006/YkJEd8CNVZC58RGSkHCa2q/afs-about-bg_40ab15f5.png"
              alt=""
              style={{
                width: '100%',
                height: '100%',
                objectFit: 'cover',
                objectPosition: 'center',
                filter: 'saturate(0.35) brightness(0.65) contrast(1.1)',
                opacity: 0.85,
              }}
            />
            {/* Gradient fade left edge */}
            <div style={{
              position: 'absolute',
              inset: 0,
              background: 'linear-gradient(to right, #0a151e 0%, transparent 40%)',
            }} />
          </div>

          {/* Top + bottom edge fades */}
          <div style={{
            position: 'absolute',
            inset: 0,
            background: 'linear-gradient(to bottom, #000000 0%, transparent 15%, transparent 85%, #000000 100%)',
            pointerEvents: 'none',
            zIndex: 1,
          }} />
        </div>

          {/* ── SKILLS ─────────────────────────────────── */}
        <section className="skills-section">
          <div className="skills-grid">
            <div>
              <h2 className="skills-sidebar-title reveal">
                Skills &<br />Expertise
              </h2>
              {/* Animated vertical line */}
              <div style={{
                width: '1px',
                height: '0',
                background: 'rgba(255,255,255,0.15)',
                marginTop: '3rem',
                animation: 'growLine 1.2s cubic-bezier(0.16,1,0.3,1) 0.4s forwards',
              }} />
            </div>
            <div className="skills-content">
              {SKILLS.map((cat, i) => (
                <div
                  key={cat.label}
                  className={`skill-category reveal reveal-delay-${i + 1}`}
                  style={{ animationDelay: `${i * 0.12}s` }}
                >
                  <div className="skill-category-label">{cat.label}</div>
                  <div className="skill-tags">
                    {cat.tags.map((tag, j) => (
                      <span
                        key={tag}
                        className="skill-tag"
                        style={{
                          animationDelay: `${i * 0.1 + j * 0.04}s`,
                          opacity: 0,
                          animation: 'fadeTagIn 0.5s ease forwards',
                        }}
                      >{tag}</span>
                    ))}
                  </div>
                </div>
              ))}
            </div>
          </div>
        </section>

        {/* ── CERTIFICATIONS ───────────────────────────── */}
        <section className="certifications-section">
          <div className="certifications-header">
            <h2 className="certifications-title reveal">Certifications</h2>
            <p className="certifications-intro reveal reveal-delay-1">
              Professional credentials spanning data analytics, digital marketing, and strategic communications.
              Click any certificate to view the credential.
            </p>
          </div>
          <div className="certifications-grid">
            {CERTIFICATIONS.map((cert, i) => (
              <div key={cert.title} className={`cert-card reveal reveal-delay-${(i % 4) + 1}`}>
                <div className="cert-card-accent" />
                <div className="cert-card-issuer">{cert.issuer}</div>
                <div className="cert-card-title">{cert.title}</div>
                <div className="cert-card-date">{cert.date}</div>
                {cert.link !== '#' ? (
                  <a href={cert.link} target="_blank" rel="noreferrer" className="cert-card-link">
                    View Certificate ↗
                  </a>
                ) : (
                  <span className="cert-card-link-placeholder">Link coming soon</span>
                )}
              </div>
            ))}
          </div>
        </section>

        {/* ── PARTICLE SPLASH DIVIDER ───────────────────── */}
        <div style={{ position: 'relative', height: '50vh', overflow: 'hidden', background: '#000' }}>
          <img
            src="https://d2xsxph8kpxj0f.cloudfront.net/310519663529869006/YkJEd8CNVZC58RGSkHCa2q/afs-divider-splash_4c4fa961.png"
            alt=""
            style={{
              width: '100%',
              height: '100%',
              objectFit: 'cover',
              objectPosition: 'center',
              opacity: 0.75,
            }}
          />
          <div style={{
            position: 'absolute',
            inset: 0,
            background: 'linear-gradient(to bottom, #000000 0%, transparent 25%, transparent 75%, #000000 100%)',
          }} />
        </div>

        {/* ── CONTACT ──────────────────────────────────── */}
        <section id="contact" className="contact-section">
          <h2 className="contact-title reveal">
            Let's<br />
            Work<br />
            Together
          </h2>

          <div className="contact-links reveal reveal-delay-1">
            <a href="mailto:afs@example.com" className="contact-link">
              <span className="contact-link-label">Email</span>
              afs@example.com
            </a>
            <a href="https://linkedin.com" target="_blank" rel="noreferrer" className="contact-link">
              <span className="contact-link-label">LinkedIn</span>
              /in/audrey-freya-shitamoto
            </a>
            <a href="https://instagram.com/audreyfrey.a" target="_blank" rel="noreferrer" className="contact-link">
              <span className="contact-link-label">Instagram</span>
              @audreyfrey.a
            </a>
            <a href="#" className="contact-link">
              <span className="contact-link-label">Resume</span>
              Download PDF ↗
            </a>
          </div>
        </section>

        {/* ── FOOTER ───────────────────────────────────── */}
        <footer className="afs-footer">
          <span className="footer-text">© 2025 Audrey Freya Shitamoto</span>
          <span className="footer-text">New York, NY</span>
          <span className="footer-text">All Rights Reserved</span>
        </footer>
      </main>
    </div>
  );
}

// ─── Intro Canvas (particle burst for intro screen) ──────────

function IntroCanvas() {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const frameRef = useRef(0);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    if (!ctx) return;

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const W = canvas.width;
    const H = canvas.height;
    const cx = W / 2;
    const cy = H / 2;

    interface Ray {
      angle: number;
      length: number;
      maxLength: number;
      speed: number;
      opacity: number;
      width: number;
    }

    const rays: Ray[] = Array.from({ length: 60 }, () => ({
      angle: Math.random() * Math.PI * 2,
      length: 0,
      maxLength: Math.random() * 200 + 80,
      speed: Math.random() * 1.5 + 0.5,
      opacity: Math.random() * 0.3 + 0.05,
      width: Math.random() * 0.8 + 0.2,
    }));

    interface Dot {
      x: number;
      y: number;
      vx: number;
      vy: number;
      size: number;
      opacity: number;
    }

    const dots: Dot[] = Array.from({ length: 80 }, () => {
      const angle = Math.random() * Math.PI * 2;
      const speed = Math.random() * 0.8 + 0.2;
      return {
        x: cx,
        y: cy,
        vx: Math.cos(angle) * speed,
        vy: Math.sin(angle) * speed,
        size: Math.random() * 1.5 + 0.3,
        opacity: Math.random() * 0.5 + 0.1,
      };
    });

    let t = 0;

    const draw = () => {
      frameRef.current = requestAnimationFrame(draw);
      t++;

      ctx.clearRect(0, 0, W, H);

      // Rays from center
      rays.forEach(ray => {
        ray.length = Math.min(ray.length + ray.speed, ray.maxLength);
        const x2 = cx + Math.cos(ray.angle) * ray.length;
        const y2 = cy + Math.sin(ray.angle) * ray.length;
        ctx.beginPath();
        ctx.moveTo(cx, cy);
        ctx.lineTo(x2, y2);
        ctx.strokeStyle = `rgba(255,255,255,${ray.opacity})`;
        ctx.lineWidth = ray.width;
        ctx.stroke();
      });

      // Floating dots
      dots.forEach(dot => {
        dot.x += dot.vx;
        dot.y += dot.vy;
        dot.vx *= 0.998;
        dot.vy *= 0.998;
        ctx.beginPath();
        ctx.arc(dot.x, dot.y, dot.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255,255,255,${dot.opacity * 0.6})`;
        ctx.fill();
      });

      // Central glow
      const grad = ctx.createRadialGradient(cx, cy, 0, cx, cy, 80);
      grad.addColorStop(0, `rgba(255,255,255,${0.04 + Math.sin(t * 0.03) * 0.02})`);
      grad.addColorStop(1, 'rgba(255,255,255,0)');
      ctx.fillStyle = grad;
      ctx.fillRect(0, 0, W, H);
    };

    frameRef.current = requestAnimationFrame(draw);

    return () => cancelAnimationFrame(frameRef.current);
  }, []);

  return (
    <canvas
      ref={canvasRef}
      style={{ position: 'absolute', inset: 0, width: '100%', height: '100%' }}
    />
  );
}
