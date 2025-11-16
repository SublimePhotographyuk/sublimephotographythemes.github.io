import React, { useState } from "react";
import { motion, AnimatePresence } from "framer-motion";

// Theme Showcase - single-file React component
// Requirements: Tailwind CSS + Framer Motion installed
// Install: npm i framer-motion
// Tailwind: follow tailwind docs; this component uses Tailwind utility classes

const THEMES = [
  {
    id: "light",
    name: "Sunlit Minimal",
    desc: "Bright, airy, and modern — perfect for galleries and portfolios.",
    accent: "#2563EB",
    bgFrom: "#ffffff",
    bgTo: "#f8fafc",
    preview: "from-white to-slate-50",
  },
  {
    id: "dark",
    name: "Midnight",
    desc: "Deep contrast with muted accents for dramatic imagery.",
    accent: "#7c3aed",
    bgFrom: "#0f172a",
    bgTo: "#020617",
    preview: "from-slate-900 to-black",
  },
  {
    id: "neon",
    name: "Neon Pop",
    desc: "Vibrant, energetic — great for music, nightlife, and products.",
    accent: "#06b6d4",
    bgFrom: "#0f172a",
    bgTo: "#0b1220",
    preview: "from-indigo-900 to-slate-900",
  },
  {
    id: "retro",
    name: "Retro Sunset",
    desc: "Warm palettes and grainy textures for nostalgia-heavy designs.",
    accent: "#f97316",
    bgFrom: "#fff7ed",
    bgTo: "#fff1e7",
    preview: "from-amber-50 to-rose-50",
  },
  {
    id: "mono",
    name: "Monochrome",
    desc: "Elegant, reduced palette for editorial and photography.",
    accent: "#111827",
    bgFrom: "#f3f4f6",
    bgTo: "#ffffff",
    preview: "from-gray-100 to-white",
  },
];

export default function ThemeShowcase() {
  const [active, setActive] = useState(THEMES[0].id);

  const activeTheme = THEMES.find((t) => t.id === active);

  return (
    <div
      className="min-h-screen p-6 md:p-12 font-inter bg-gradient-to-b"
      style={{
        background: `linear-gradient(180deg, ${activeTheme.bgFrom} 0%, ${activeTheme.bgTo} 100%)`,
        transition: "background 300ms ease",
      }}
    >
      <div className="max-w-6xl mx-auto">
        <header className="flex items-center justify-between mb-8">
          <div>
            <h1 className="text-3xl md:text-4xl font-extrabold tracking-tight">
              Theme Studio
            </h1>
            <p className="mt-1 text-sm text-slate-600">A playful gallery of UI themes.</p>
          </div>

          <div className="flex items-center gap-3">
            <div className="text-xs text-slate-500 mr-2 hidden md:block">Preview size</div>
            <div className="flex items-center gap-2">
              <button
                onClick={() => window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches && setActive('dark')}
                className="px-3 py-1 rounded-full border text-xs"
                title="Use system dark preference"
              >
                Auto
              </button>
              <a
                className="text-xs px-3 py-1 rounded-full border bg-white/30 hover:bg-white/40"
                href="#download"
                onClick={(e) => {
                  e.preventDefault();
                  // Minimal in-browser code download example
                  const blob = new Blob([document.documentElement.outerHTML], { type: "text/html" });
                  const url = URL.createObjectURL(blob);
                  const a = document.createElement("a");
                  a.href = url;
                  a.download = `theme-showcase-${active}.html`;
                  a.click();
                  URL.revokeObjectURL(url);
                }}
              >
                Export
              </a>
            </div>
          </div>
        </header>

        <main className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Left: theme list */}
          <section className="lg:col-span-1 space-y-4">
            <div className="grid gap-3">
              {THEMES.map((t) => (
                <motion.button
                  key={t.id}
                  onClick={() => setActive(t.id)}
                  whileHover={{ scale: 1.02 }}
                  whileTap={{ scale: 0.98 }}
                  className={`group relative p-4 rounded-2xl text-left transition-shadow shadow-sm border ${
                    active === t.id ? "ring-4 ring-opacity-30" : "hover:shadow-md"
                  }`}
                  style={{
                    boxShadow: active === t.id ? `0 10px 30px -10px ${t.accent}` : undefined,
                  }}
                >
                  <div className="flex items-center justify-between">
                    <div>
                      <div className="flex items-center gap-3">
                        <div
                          className="w-10 h-10 rounded-lg flex items-center justify-center text-white font-bold"
                          style={{ background: t.accent }}
                        >
                          {t.name[0]}
                        </div>
                        <div>
                          <div className="font-semibold">{t.name}</div>
                          <div className="text-xs text-slate-500">{t.desc}</div>
                        </div>
                      </div>
                    </div>
                    <div className="text-sm text-slate-400">Preview →</div>
                  </div>
                </motion.button>
              ))}
            </div>

            <div className="mt-6 p-4 rounded-xl border bg-white/30">
              <h3 className="font-semibold">About</h3>
              <p className="text-sm text-slate-600 mt-2">
                Click a theme to try it. The preview updates live and demonstrates color palettes,
                typography, buttons, cards and sample content.
              </p>
            </div>
          </section>

          {/* Middle + Right: preview area */}
          <section className="lg:col-span-2 grid grid-rows-[auto,1fr] gap-6">
            <div className="rounded-2xl border overflow-hidden">
              <div className="flex items-center justify-between p-4 bg-white/5">
                <div className="flex items-center gap-4">
                  <div className="rounded-md p-2" style={{ background: activeTheme.accent }} />
                  <div>
                    <div className="font-semibold">{activeTheme.name}</div>
                    <div className="text-xs text-slate-500">{activeTheme.desc}</div>
                  </div>
                </div>

                <div className="flex items-center gap-3">
                  <button className="px-3 py-1 rounded-md border text-sm">Primary</button>
                  <button className="px-3 py-1 rounded-md border text-sm">Secondary</button>
                </div>
              </div>

              <div className="p-6 bg-gradient-to-b" style={{ background: `linear-gradient(180deg, rgba(255,255,255,0.03), rgba(0,0,0,0.02))` }}>
                <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                  <div className="col-span-2">
                    <div className="rounded-lg p-6 border h-48 shadow-inner" style={{ background: 'linear-gradient(90deg, rgba(255,255,255,0.02), rgba(0,0,0,0.02))' }}>
                      <div className="flex items-center justify-between">
                        <div>
                          <h2 className="text-xl font-bold">Showcase Panel</h2>
                          <p className="text-sm text-slate-500 mt-1">A live example of headings, text, and a call-to-action.</p>
                        </div>
                        <div>
                          <button className="px-4 py-2 rounded-lg font-medium" style={{ background: activeTheme.accent, color: '#fff' }}>
                            Action
                          </button>
                        </div>
                      </div>

                      <div className="mt-6 flex gap-3">
                        <div className="flex-1 rounded-lg p-3 border">
                          <div className="text-xs font-semibold">Card Title</div>
                          <div className="text-sm text-slate-500 mt-1">A short snippet showing how cards look in this theme.</div>
                        </div>
                        <div className="w-48 rounded-lg p-3 border flex items-center justify-center">Preview Box</div>
                      </div>
                    </div>
                  </div>

                  <div>
                    <div className="rounded-lg border p-4 h-48 flex flex-col justify-between">
                      <div>
                        <div className="text-xs text-slate-500">Palette</div>
                        <div className="mt-2 flex gap-2">
                          <div className="w-8 h-8 rounded" style={{ background: activeTheme.accent }} />
                          <div className="w-8 h-8 rounded" style={{ background: '#94a3b8' }} />
                          <div className="w-8 h-8 rounded" style={{ background: '#e2e8f0' }} />
                        </div>
                      </div>

                      <div className="text-xs text-slate-500">Typography
                        <div className="mt-2 text-sm font-medium">Inter / System</div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div className="rounded-2xl border p-6 bg-white/5">
              <div className="flex items-start gap-6">
                <div className="w-48">
                  <div className="text-xs text-slate-500">Live Preview</div>
                  <div className="mt-3 rounded-lg border overflow-hidden">
                    <div className="p-4" style={{ background: activeTheme.bgFrom }}>
                      <AnimatePresence mode="wait">
                        <motion.div
                          key={activeTheme.id}
                          initial={{ opacity: 0, y: 6 }}
                          animate={{ opacity: 1, y: 0 }}
                          exit={{ opacity: 0, y: -6 }}
                          transition={{ duration: 0.25 }}
                        >
                          <div className="rounded-md p-3">
                            <div className="text-sm font-semibold">{activeTheme.name}</div>
                            <div className="text-xs text-slate-500">{activeTheme.desc}</div>

                            <div className="mt-3 flex gap-2">
                              <button className="flex-1 text-xs p-2 rounded-md font-medium" style={{ background: activeTheme.accent, color: '#fff' }}>
                                Primary
                              </button>
                              <button className="text-xs p-2 rounded-md border">Ghost</button>
                            </div>

                            <div className="mt-3 text-xs text-slate-600">Sample text — quick visual check for contrast and rhythm.</div>
                          </div>
                        </motion.div>
                      </AnimatePresence>
                    </div>
                  </div>
                </div>

                <div className="flex-1">
                  <h4 className="font-semibold">Notes & usage</h4>
                  <p className="text-sm text-slate-600 mt-2">Each theme is built with simple variables: accent color, background gradient, and typographic scale. Use these as a starting point and tweak to taste.</p>

                  <div className="mt-4 grid grid-cols-2 gap-3">
                    <div className="p-3 rounded border text-sm">Buttons</div>
                    <div className="p-3 rounded border text-sm">Cards</div>
                    <div className="p-3 rounded border text-sm">Headers</div>
                    <div className="p-3 rounded border text-sm">Forms</div>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </main>

        <footer className="mt-8 text-center text-sm text-slate-500">Made with ❤️ — tweak the code and add your own themes.</footer>
      </div>
    </div>
  );
}
