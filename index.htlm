import { useState, useEffect, useRef } from "react";

// ─── THEME ───────────────────────────────────────────────────────────────────
const C = {
  bg: "#07090f", surface: "#0e1420", card: "#131c2e", cardHover: "#172035",
  border: "#1a3050", accent: "#00c2ff", gold: "#f5a623", green: "#00e676",
  red: "#ff4757", purple: "#a855f7", text: "#e2e8f0", muted: "#4a5568", sub: "#8892a4",
};

// ─── GLOBAL STORE (simulates a database in memory) ───────────────────────────
const STORE = {
  // Admin credentials (hardcoded)
  admin: { user: "admin", password: "nexo2025" },

  // Registered vendors: { id, email, password, name, province, whatsapp, plan, status, products, trialEnd }
  vendors: [
    { id: "v1", email: "tienda@gmail.com", password: "123456", name: "Tienda El Palacio", province: "La Habana", whatsapp: "+53 52000001", plan: "mensual", status: "activo", trialEnd: null,
      products: [
        { id: "p1", name: "iPhone 13 Pro 256GB", price: 75000, priceStr: "75,000", category: "Tecnología", province: "La Habana", stock: 2, photos: 4, views: 312, clicks: 47, commission: 5, description: "iPhone 13 Pro en excelente estado, caja original." },
        { id: "p2", name: "Nevera Samsung 400L", price: 120000, priceStr: "120,000", category: "Electrodomésticos", province: "La Habana", stock: 1, photos: 6, views: 198, clicks: 23, commission: 0, description: "Nevera Samsung casi nueva, fría al máximo." },
      ]
    },
    { id: "v2", email: "techcuba@gmail.com", password: "123456", name: "Tech Cuba Store", province: "Santiago de Cuba", whatsapp: "+53 52000002", plan: "quincenal", status: "activo", trialEnd: null,
      products: [
        { id: "p3", name: "Smart TV Samsung 55\"", price: 95000, priceStr: "95,000", category: "Tecnología", province: "Santiago de Cuba", stock: 1, photos: 5, views: 445, clicks: 61, commission: 8, description: "Smart TV 4K, Netflix, YouTube integrado." },
        { id: "p4", name: "Laptop HP i5 8GB", price: 55000, priceStr: "55,000", category: "Tecnología", province: "Santiago de Cuba", stock: 2, photos: 4, views: 210, clicks: 30, commission: 5, description: "Laptop HP Core i5, 8GB RAM, 256GB SSD." },
      ]
    },
  ],

  // Registered affiliates: { id, email, password, name, earnings, links }
  affiliates: [
    { id: "a1", email: "promo@gmail.com", password: "123456", name: "Carlos Promotor", earnings: 11350, links: 12, sales: 3 },
  ],

  // Registered buyers: { id, email, password, name, favorites }
  buyers: [
    { id: "b1", email: "comprador@gmail.com", password: "123456", name: "Ana García", favorites: [] },
  ],

  payments: [
    { id: 1, vendorId: "v2", user: "Tech Cuba Store", amount: "1,000", plan: "quincenal", date: "2025-04-04 09:23", status: "pendiente" },
    { id: 2, vendorId: "v1", user: "Tienda El Palacio", amount: "2,000", plan: "mensual", date: "2025-04-02 11:05", status: "aprobado" },
  ],

  webBlocks: [
    { id: 1, type: "banner", title: "¡Bienvenido a NexoMercado!", subtitle: "El marketplace #1 de Cuba 🇨🇺", content: "", link: "", color: C.accent, priority: "alta", active: true },
    { id: 2, type: "promo", title: "Semana del Vendedor", subtitle: "Plan Mensual con 20% OFF esta semana", content: "", link: "", color: C.gold, priority: "normal", active: true },
  ],

  bankInfo: { card: "9225 1234 5678 9012", name: "Maikel González", bank: "Banco Metropolitano" },
};

// All products across all vendors (for buyer search)
function getAllProducts() {
  return STORE.vendors.flatMap(v =>
    v.products.map(p => ({ ...p, vendorName: v.name, vendorWA: v.whatsapp, vendorProvince: v.province }))
  );
}

// ─── UTILS ───────────────────────────────────────────────────────────────────
const PROVINCES = ["Todas", "La Habana", "Santiago de Cuba", "Matanzas", "Holguín", "Camagüey", "Villa Clara", "Granma", "Guantánamo", "Las Tunas", "Pinar del Río", "Cienfuegos", "Sancti Spíritus", "Ciego de Ávila", "Artemisa", "Mayabeque", "Isla de la Juventud"];
const CATEGORIES = ["Todas", "Tecnología", "Electrodomésticos", "Ropa", "Hogar", "Deportes", "Autos", "Inmuebles", "Alimentos", "Servicios", "Otros"];
const BLOCK_TYPES = [
  { value: "banner", label: "🎨 Banner / Cabecera" }, { value: "promo", label: "🔥 Promoción" },
  { value: "aviso", label: "📢 Aviso Importante" }, { value: "noticia", label: "📰 Noticia" },
  { value: "evento", label: "🎉 Evento" }, { value: "texto", label: "📝 Texto Libre" },
  { value: "enlace", label: "🔗 Enlace Externo" }, { value: "html", label: "💻 HTML Personalizado" },
];
const BLOCK_ICONS = { banner: "🎨", promo: "🔥", aviso: "📢", noticia: "📰", evento: "🎉", texto: "📝", enlace: "🔗", html: "💻" };
const PALETTE = [C.accent, C.gold, C.green, C.red, C.purple, "#ff6b35", "#00b4d8", "#e91e63"];

function Badge({ label, color = C.accent }) {
  return <span style={{ background: color + "22", color, border: `1px solid ${color}44`, borderRadius: 6, padding: "2px 10px", fontSize: 11, fontWeight: 700, whiteSpace: "nowrap" }}>{label}</span>;
}

function StatCard({ icon, label, value, sub, color = C.accent }) {
  return (
    <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, padding: "18px 20px", flex: 1, minWidth: 120, position: "relative", overflow: "hidden" }}>
      <div style={{ fontSize: 18, marginBottom: 5 }}>{icon}</div>
      <div style={{ fontSize: 24, fontWeight: 800, color, fontFamily: "monospace" }}>{value}</div>
      <div style={{ fontSize: 12, color: C.sub, marginTop: 2 }}>{label}</div>
      {sub && <div style={{ fontSize: 11, color: C.muted, marginTop: 2 }}>{sub}</div>}
      <div style={{ position: "absolute", top: -16, right: -16, width: 60, height: 60, borderRadius: "50%", background: color + "0d" }} />
    </div>
  );
}

function Btn({ children, onClick, color = C.accent, small, ghost, full, icon, disabled }) {
  const [h, setH] = useState(false);
  return (
    <button onClick={onClick} disabled={disabled} onMouseEnter={() => setH(true)} onMouseLeave={() => setH(false)}
      style={{ background: disabled ? C.muted + "33" : ghost ? "transparent" : h ? color : color + "dd", color: disabled ? C.muted : ghost ? color : "#fff", border: `1.5px solid ${disabled ? C.muted : color}`, borderRadius: 9, padding: small ? "6px 14px" : "11px 22px", fontWeight: 700, fontSize: small ? 12 : 14, cursor: disabled ? "not-allowed" : "pointer", transition: "all .15s", width: full ? "100%" : undefined, display: "inline-flex", alignItems: "center", justifyContent: "center", gap: 6, whiteSpace: "nowrap" }}>
      {icon && <span>{icon}</span>}{children}
    </button>
  );
}

function Inp({ label, value, onChange, placeholder, type = "text", error }) {
  const [focus, setFocus] = useState(false);
  return (
    <div style={{ marginBottom: 14 }}>
      {label && <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 5, fontWeight: 600 }}>{label}</label>}
      <input type={type} value={value} onChange={e => onChange(e.target.value)} placeholder={placeholder}
        onFocus={() => setFocus(true)} onBlur={() => setFocus(false)}
        style={{ width: "100%", background: C.bg, border: `1.5px solid ${error ? C.red : focus ? C.accent : C.border}`, borderRadius: 9, padding: "11px 14px", color: C.text, fontSize: 14, outline: "none", boxSizing: "border-box", transition: "border .15s" }} />
      {error && <div style={{ fontSize: 11, color: C.red, marginTop: 4 }}>⚠️ {error}</div>}
    </div>
  );
}

function Sel({ label, value, onChange, options }) {
  return (
    <div style={{ marginBottom: 14 }}>
      {label && <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 5, fontWeight: 600 }}>{label}</label>}
      <select value={value} onChange={e => onChange(e.target.value)}
        style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "11px 14px", color: C.text, fontSize: 14, outline: "none", boxSizing: "border-box" }}>
        {options.map(o => <option key={o} value={o}>{o}</option>)}
      </select>
    </div>
  );
}

function Textarea({ label, value, onChange, placeholder, rows = 3 }) {
  return (
    <div style={{ marginBottom: 14 }}>
      {label && <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 5, fontWeight: 600 }}>{label}</label>}
      <textarea value={value} onChange={e => onChange(e.target.value)} placeholder={placeholder} rows={rows}
        style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "11px 14px", color: C.text, fontSize: 14, outline: "none", resize: "vertical", boxSizing: "border-box", fontFamily: "inherit" }} />
    </div>
  );
}

function Sidebar({ tabs, active, onSelect, roleLabel, color, userName, onLogout }) {
  return (
    <div style={{ width: 210, background: C.surface, borderRight: `1px solid ${C.border}`, display: "flex", flexDirection: "column", flexShrink: 0 }}>
      <div style={{ padding: "20px 16px", borderBottom: `1px solid ${C.border}30` }}>
        <div style={{ fontSize: 10, color: C.muted, letterSpacing: 2, textTransform: "uppercase", marginBottom: 4 }}>Panel de</div>
        <div style={{ fontSize: 14, fontWeight: 800, color }}>{roleLabel}</div>
        <div style={{ fontSize: 12, color: C.muted, marginTop: 4, overflow: "hidden", textOverflow: "ellipsis", whiteSpace: "nowrap" }}>👤 {userName}</div>
      </div>
      <div style={{ flex: 1, padding: "10px 0", overflowY: "auto" }}>
        {tabs.map(t => (
          <div key={t.id} onClick={() => onSelect(t.id)} style={{ padding: "11px 18px", cursor: "pointer", background: active === t.id ? color + "15" : "transparent", borderLeft: active === t.id ? `3px solid ${color}` : "3px solid transparent", color: active === t.id ? color : C.sub, fontSize: 13, fontWeight: active === t.id ? 700 : 400, display: "flex", alignItems: "center", gap: 9, transition: "all .13s" }}>
            {t.icon} {t.label}
          </div>
        ))}
      </div>
      <div style={{ padding: "14px 16px", borderTop: `1px solid ${C.border}30` }}>
        <div style={{ fontSize: 11, color: C.muted, marginBottom: 6 }}>💬 +53 53215857</div>
        <Btn small ghost color={C.red} full onClick={onLogout} icon="→">Cerrar Sesión</Btn>
      </div>
    </div>
  );
}

function Topbar({ onBack }) {
  return (
    <div style={{ background: C.surface, borderBottom: `1px solid ${C.border}`, padding: "11px 22px", display: "flex", alignItems: "center", justifyContent: "space-between", flexShrink: 0 }}>
      <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
        <span style={{ fontSize: 20 }}>🌐</span>
        <span style={{ fontSize: 17, fontWeight: 900, fontFamily: "monospace", color: C.text }}>Nexo<span style={{ color: C.accent }}>Mercado</span></span>
      </div>
      {onBack && <Btn small ghost color={C.muted} onClick={onBack} icon="←">Inicio</Btn>}
    </div>
  );
}

// ─── COUNTDOWN ───────────────────────────────────────────────────────────────
function Countdown({ seconds }) {
  const [left, setLeft] = useState(seconds);
  useEffect(() => { const t = setInterval(() => setLeft(p => Math.max(0, p - 1)), 1000); return () => clearInterval(t); }, []);
  const h = Math.floor(left / 3600), m = Math.floor((left % 3600) / 60), s = left % 60;
  const pct = (left / seconds) * 100;
  const col = pct > 50 ? C.green : pct > 20 ? C.gold : C.red;
  return (
    <div style={{ background: C.card, border: `1px solid ${col}55`, borderRadius: 14, padding: 16, textAlign: "center" }}>
      <div style={{ fontSize: 10, color: C.muted, marginBottom: 6, letterSpacing: 1, textTransform: "uppercase" }}>⏱ Prueba termina en</div>
      <div style={{ fontSize: 30, fontFamily: "monospace", fontWeight: 800, color: col, letterSpacing: 2 }}>
        {String(h).padStart(2, "0")}:{String(m).padStart(2, "0")}:{String(s).padStart(2, "0")}
      </div>
      <div style={{ background: C.bg, borderRadius: 4, height: 4, margin: "10px 0", overflow: "hidden" }}>
        <div style={{ height: "100%", width: `${pct}%`, background: col, transition: "width 1s linear" }} />
      </div>
      <Btn color={C.gold} full small>🚀 Activar Plan Ahora</Btn>
    </div>
  );
}

// ─── AUTH SCREENS ─────────────────────────────────────────────────────────────

function AuthCard({ title, subtitle, children }) {
  return (
    <div style={{ minHeight: "100vh", background: C.bg, display: "flex", alignItems: "center", justifyContent: "center", padding: 20, fontFamily: "'Segoe UI', sans-serif" }}>
      <div style={{ width: "100%", maxWidth: 420 }}>
        <div style={{ textAlign: "center", marginBottom: 32 }}>
          <div style={{ fontSize: 40, marginBottom: 8 }}>🌐</div>
          <div style={{ fontSize: 28, fontWeight: 900, fontFamily: "monospace", color: C.text }}>Nexo<span style={{ color: C.accent }}>Mercado</span></div>
          <div style={{ fontSize: 13, color: C.muted, marginTop: 4 }}>El marketplace profesional de Cuba 🇨🇺</div>
        </div>
        <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 20, padding: "30px 28px" }}>
          <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 4 }}>{title}</div>
          <div style={{ fontSize: 13, color: C.muted, marginBottom: 24 }}>{subtitle}</div>
          {children}
        </div>
        <div style={{ textAlign: "center", marginTop: 20, fontSize: 12, color: C.muted }}>
          💬 <a href="https://wa.me/5353215857" style={{ color: C.accent, textDecoration: "none" }}>+53 53215857</a>
          {" · "}
          <a href="mailto:maikolgb07@gmail.com" style={{ color: C.accent, textDecoration: "none" }}>maikolgb07@gmail.com</a>
        </div>
      </div>
    </div>
  );
}

// Admin Login
function AdminLogin({ onSuccess }) {
  const [user, setUser] = useState("");
  const [pass, setPass] = useState("");
  const [err, setErr] = useState("");

  const login = () => {
    if (user === STORE.admin.user && pass === STORE.admin.password) { onSuccess(); }
    else { setErr("Usuario o contraseña incorrectos"); }
  };

  return (
    <AuthCard title="🔴 Panel Administrador" subtitle="Acceso exclusivo — credenciales requeridas">
      <Inp label="Usuario" value={user} onChange={setUser} placeholder="admin" error={err && " "} />
      <Inp label="Contraseña" type="password" value={pass} onChange={setPass} placeholder="••••••••" error={err} />
      <Btn full color={C.red} onClick={login}>Entrar como Admin</Btn>
    </AuthCard>
  );
}

// Vendor Login / Register
function VendorAuth({ onSuccess }) {
  const [mode, setMode] = useState("login"); // login | register
  const [email, setEmail] = useState("");
  const [pass, setPass] = useState("");
  const [name, setName] = useState("");
  const [province, setProvince] = useState("La Habana");
  const [whatsapp, setWhatsapp] = useState("");
  const [err, setErr] = useState("");

  const login = () => {
    const v = STORE.vendors.find(x => x.email === email && x.password === pass);
    if (v) onSuccess(v);
    else setErr("Correo o contraseña incorrectos");
  };

  const register = () => {
    if (!name || !email || !pass || !whatsapp) { setErr("Completa todos los campos"); return; }
    if (STORE.vendors.find(x => x.email === email)) { setErr("Este correo ya está registrado"); return; }
    const newV = {
      id: "v" + Date.now(), email, password: pass, name, province, whatsapp,
      plan: "prueba", status: "prueba",
      trialEnd: Date.now() + 3 * 24 * 60 * 60 * 1000,
      products: [],
    };
    STORE.vendors.push(newV);
    onSuccess(newV);
  };

  return (
    <AuthCard title="🔵 Panel Vendedor" subtitle={mode === "login" ? "Entra a tu tienda" : "Crea tu cuenta de vendedor gratis"}>
      {mode === "register" && (
        <>
          <Inp label="Nombre de tu Tienda / Negocio" value={name} onChange={setName} placeholder="Ej: Tienda El Sol" />
          <Inp label="WhatsApp de Contacto" value={whatsapp} onChange={setWhatsapp} placeholder="+53 5XXXXXXX" />
          <Sel label="Provincia" value={province} onChange={setProvince} options={PROVINCES.slice(1)} />
        </>
      )}
      <Inp label="Correo Electrónico" type="email" value={email} onChange={setEmail} placeholder="tu@correo.com" />
      <Inp label="Contraseña" type="password" value={pass} onChange={setPass} placeholder="••••••••" error={err} />
      {mode === "login"
        ? <Btn full color={C.accent} onClick={login}>Entrar a Mi Tienda</Btn>
        : <Btn full color={C.green} onClick={register}>✅ Crear Mi Cuenta Gratis</Btn>
      }
      <div style={{ textAlign: "center", marginTop: 16, fontSize: 13, color: C.muted }}>
        {mode === "login"
          ? <>¿No tienes cuenta? <span style={{ color: C.accent, cursor: "pointer", fontWeight: 700 }} onClick={() => { setMode("register"); setErr(""); }}>Regístrate gratis</span></>
          : <>¿Ya tienes cuenta? <span style={{ color: C.accent, cursor: "pointer", fontWeight: 700 }} onClick={() => { setMode("login"); setErr(""); }}>Inicia sesión</span></>
        }
      </div>
      {mode === "login" && (
        <div style={{ marginTop: 16, background: C.bg, borderRadius: 10, padding: 12, border: `1px dashed ${C.border}` }}>
          <div style={{ fontSize: 11, color: C.muted }}>🧪 Cuenta de prueba: <strong style={{ color: C.sub }}>tienda@gmail.com</strong> / <strong style={{ color: C.sub }}>123456</strong></div>
        </div>
      )}
    </AuthCard>
  );
}

// Affiliate Auth
function AffiliateAuth({ onSuccess }) {
  const [mode, setMode] = useState("login");
  const [email, setEmail] = useState("");
  const [pass, setPass] = useState("");
  const [name, setName] = useState("");
  const [err, setErr] = useState("");

  const login = () => {
    const a = STORE.affiliates.find(x => x.email === email && x.password === pass);
    if (a) onSuccess(a); else setErr("Correo o contraseña incorrectos");
  };
  const register = () => {
    if (!name || !email || !pass) { setErr("Completa todos los campos"); return; }
    if (STORE.affiliates.find(x => x.email === email)) { setErr("Este correo ya está registrado"); return; }
    const a = { id: "a" + Date.now(), email, password: pass, name, earnings: 0, links: 0, sales: 0 };
    STORE.affiliates.push(a);
    onSuccess(a);
  };

  return (
    <AuthCard title="🟢 Panel de Afiliado" subtitle={mode === "login" ? "Accede a tu cuenta de promotor" : "Únete y empieza a ganar comisiones"}>
      {mode === "register" && <Inp label="Tu Nombre Completo" value={name} onChange={setName} placeholder="Ej: Carlos Rodríguez" />}
      <Inp label="Correo Electrónico" type="email" value={email} onChange={setEmail} placeholder="tu@correo.com" />
      <Inp label="Contraseña" type="password" value={pass} onChange={setPass} placeholder="••••••••" error={err} />
      {mode === "login"
        ? <Btn full color={C.green} onClick={login}>Entrar como Afiliado</Btn>
        : <Btn full color={C.green} onClick={register}>✅ Registrarme Gratis</Btn>
      }
      <div style={{ textAlign: "center", marginTop: 16, fontSize: 13, color: C.muted }}>
        {mode === "login"
          ? <>¿No tienes cuenta? <span style={{ color: C.green, cursor: "pointer", fontWeight: 700 }} onClick={() => { setMode("register"); setErr(""); }}>Regístrate</span></>
          : <>¿Ya tienes cuenta? <span style={{ color: C.green, cursor: "pointer", fontWeight: 700 }} onClick={() => { setMode("login"); setErr(""); }}>Inicia sesión</span></>
        }
      </div>
    </AuthCard>
  );
}

// Buyer Auth
function BuyerAuth({ onSuccess }) {
  const [mode, setMode] = useState("login");
  const [email, setEmail] = useState("");
  const [pass, setPass] = useState("");
  const [name, setName] = useState("");
  const [err, setErr] = useState("");

  const login = () => {
    const b = STORE.buyers.find(x => x.email === email && x.password === pass);
    if (b) onSuccess(b); else setErr("Correo o contraseña incorrectos");
  };
  const register = () => {
    if (!name || !email || !pass) { setErr("Completa todos los campos"); return; }
    if (STORE.buyers.find(x => x.email === email)) { setErr("Este correo ya está registrado"); return; }
    const b = { id: "b" + Date.now(), email, password: pass, name, favorites: [] };
    STORE.buyers.push(b);
    onSuccess(b);
  };

  return (
    <AuthCard title="🟡 Portal del Comprador" subtitle={mode === "login" ? "Entra para guardar favoritos y más" : "Crea tu cuenta gratis"}>
      {mode === "register" && <Inp label="Tu Nombre" value={name} onChange={setName} placeholder="Ej: Ana García" />}
      <Inp label="Correo Electrónico" type="email" value={email} onChange={setEmail} placeholder="tu@correo.com" />
      <Inp label="Contraseña" type="password" value={pass} onChange={setPass} placeholder="••••••••" error={err} />
      {mode === "login"
        ? <Btn full color={C.gold} onClick={login}>Entrar a Comprar</Btn>
        : <Btn full color={C.gold} onClick={register}>✅ Crear Mi Cuenta</Btn>
      }
      <div style={{ textAlign: "center", marginTop: 16, fontSize: 13, color: C.muted }}>
        {mode === "login"
          ? <>¿No tienes cuenta? <span style={{ color: C.gold, cursor: "pointer", fontWeight: 700 }} onClick={() => { setMode("register"); setErr(""); }}>Regístrate</span></>
          : <>¿Ya tienes cuenta? <span style={{ color: C.gold, cursor: "pointer", fontWeight: 700 }} onClick={() => { setMode("login"); setErr(""); }}>Inicia sesión</span></>
        }
      </div>
      <div style={{ marginTop: 16, background: C.bg, borderRadius: 10, padding: 12, border: `1px dashed ${C.border}` }}>
        <div style={{ fontSize: 11, color: C.muted }}>🧪 Prueba: <strong style={{ color: C.sub }}>comprador@gmail.com</strong> / <strong style={{ color: C.sub }}>123456</strong></div>
      </div>
    </AuthCard>
  );
}

// ═══════════════════════════════════════════════════════════════════════════
// ADMIN PANEL
// ═══════════════════════════════════════════════════════════════════════════
function AdminPanel({ onLogout }) {
  const [tab, setTab] = useState("dashboard");
  const [payments, setPayments] = useState(STORE.payments);
  const [bankInfo, setBankInfo] = useState({ ...STORE.bankInfo });
  const [webBlocks, setWebBlocks] = useState(STORE.webBlocks);
  const [showAdd, setShowAdd] = useState(false);
  const [editId, setEditId] = useState(null);
  const [nb, setNb] = useState({ type: "banner", title: "", subtitle: "", content: "", link: "", color: C.accent, priority: "normal" });
  const [saved, setSaved] = useState(false);

  const tabs = [
    { id: "dashboard", icon: "📊", label: "Dashboard" },
    { id: "pagos", icon: "💳", label: "Validar Pagos" },
    { id: "usuarios", icon: "👥", label: "Vendedores" },
    { id: "cobro", icon: "⚙️", label: "Config. Cobro" },
    { id: "web", icon: "🌐", label: "Editor Web" },
  ];

  const approve = id => {
    const p = payments.find(x => x.id === id);
    if (p) {
      const v = STORE.vendors.find(x => x.id === p.vendorId);
      if (v) { v.plan = p.plan; v.status = "activo"; }
    }
    setPayments(ps => ps.map(x => x.id === id ? { ...x, status: "aprobado" } : x));
  };

  const submitBlock = () => {
    if (!nb.title) return;
    if (editId) {
      setWebBlocks(b => b.map(x => x.id === editId ? { ...nb, id: editId, active: x.active } : x));
      setEditId(null);
    } else {
      setWebBlocks(b => [...b, { ...nb, id: Date.now(), active: true }]);
    }
    setNb({ type: "banner", title: "", subtitle: "", content: "", link: "", color: C.accent, priority: "normal" });
    setShowAdd(false); setSaved(true); setTimeout(() => setSaved(false), 2500);
  };

  const startEdit = block => { setNb({ ...block }); setEditId(block.id); setShowAdd(true); };
  const toggleBlock = id => setWebBlocks(b => b.map(x => x.id === id ? { ...x, active: !x.active } : x));
  const removeBlock = id => setWebBlocks(b => b.filter(x => x.id !== id));
  const moveBlock = (id, dir) => setWebBlocks(b => {
    const idx = b.findIndex(x => x.id === id), arr = [...b], swap = idx + dir;
    if (swap < 0 || swap >= arr.length) return arr;
    [arr[idx], arr[swap]] = [arr[swap], arr[idx]]; return arr;
  });

  return (
    <div style={{ display: "flex", height: "100%", overflow: "hidden" }}>
      <Sidebar tabs={tabs} active={tab} onSelect={setTab} roleLabel="🔴 Administrador" color={C.red} userName="Maikel González" onLogout={onLogout} />
      <div style={{ flex: 1, padding: 26, overflowY: "auto" }}>

        {tab === "dashboard" && (<>
          <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>📊 Resumen General</div>
          <div style={{ display: "flex", gap: 12, flexWrap: "wrap", marginBottom: 24 }}>
            <StatCard icon="🏪" label="Vendedores Activos" value={STORE.vendors.filter(v => v.status === "activo").length} color={C.green} />
            <StatCard icon="⏳" label="En Prueba" value={STORE.vendors.filter(v => v.status === "prueba").length} color={C.gold} />
            <StatCard icon="💳" label="Pagos Pendientes" value={payments.filter(p => p.status === "pendiente").length} color={C.red} />
            <StatCard icon="💰" label="Ingresos Mes" value="$184K" sub="CUP" color={C.accent} />
          </div>
          <div style={{ fontSize: 14, fontWeight: 700, color: C.text, marginBottom: 12 }}>📋 Vendedores Registrados</div>
          <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, overflow: "hidden" }}>
            {STORE.vendors.map((v, i) => (
              <div key={v.id} style={{ display: "flex", alignItems: "center", gap: 12, padding: "13px 18px", borderBottom: i < STORE.vendors.length - 1 ? `1px solid ${C.border}30` : "none" }}>
                <div style={{ width: 34, height: 34, borderRadius: "50%", background: C.accent + "22", display: "flex", alignItems: "center", justifyContent: "center" }}>🏪</div>
                <div style={{ flex: 1 }}>
                  <div style={{ fontSize: 14, fontWeight: 600, color: C.text }}>{v.name}</div>
                  <div style={{ fontSize: 12, color: C.muted }}>{v.province} · {v.products.length} productos · {v.email}</div>
                </div>
                <Badge label={v.status} color={v.status === "activo" ? C.green : v.status === "prueba" ? C.gold : C.red} />
                <Badge label={v.plan} color={v.plan === "mensual" ? C.gold : v.plan === "quincenal" ? C.accent : C.muted} />
              </div>
            ))}
          </div>
        </>)}

        {tab === "pagos" && (<>
          <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>💳 Validación de Pagos</div>
          <div style={{ display: "flex", flexDirection: "column", gap: 12 }}>
            {payments.map(p => (
              <div key={p.id} style={{ background: C.card, border: `1px solid ${p.status === "pendiente" ? C.gold + "55" : C.border}`, borderRadius: 14, padding: 18, display: "flex", gap: 14, alignItems: "center" }}>
                <div style={{ width: 64, height: 52, background: C.bg, borderRadius: 10, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 26, border: `1px dashed ${C.border}` }}>🖼️</div>
                <div style={{ flex: 1 }}>
                  <div style={{ fontSize: 15, fontWeight: 700, color: C.text }}>{p.user}</div>
                  <div style={{ fontSize: 13, color: C.muted }}>Plan {p.plan} · <strong style={{ color: C.accent }}>${p.amount} CUP</strong></div>
                  <div style={{ fontSize: 12, color: C.muted }}>📅 {p.date}</div>
                </div>
                {p.status === "pendiente" ? <Btn color={C.green} onClick={() => approve(p.id)} icon="✅">Aprobar</Btn> : <Badge label="✅ Aprobado" color={C.green} />}
              </div>
            ))}
          </div>
        </>)}

        {tab === "usuarios" && (<>
          <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>👥 Todos los Vendedores</div>
          <div style={{ display: "flex", gap: 12, flexWrap: "wrap", marginBottom: 18 }}>
            <StatCard icon="✅" label="Activos" value={STORE.vendors.filter(v => v.status === "activo").length} color={C.green} />
            <StatCard icon="⏳" label="Prueba" value={STORE.vendors.filter(v => v.status === "prueba").length} color={C.gold} />
            <StatCard icon="👥" label="Total" value={STORE.vendors.length} color={C.accent} />
          </div>
          <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, overflow: "hidden" }}>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 120px 110px 80px", padding: "10px 18px", background: C.bg, fontSize: 11, color: C.muted, fontWeight: 700, letterSpacing: 1, textTransform: "uppercase" }}>
              <div>Negocio</div><div>Plan</div><div>Estado</div><div>Prods</div>
            </div>
            {STORE.vendors.map(v => (
              <div key={v.id} style={{ display: "grid", gridTemplateColumns: "1fr 120px 110px 80px", padding: "12px 18px", borderTop: `1px solid ${C.border}20`, alignItems: "center" }}>
                <div>
                  <div style={{ fontSize: 14, fontWeight: 600, color: C.text }}>{v.name}</div>
                  <div style={{ fontSize: 11, color: C.muted }}>{v.province}</div>
                </div>
                <Badge label={v.plan} color={v.plan === "mensual" ? C.gold : v.plan === "quincenal" ? C.accent : C.muted} />
                <Badge label={v.status} color={v.status === "activo" ? C.green : v.status === "prueba" ? C.gold : C.red} />
                <div style={{ fontSize: 13, color: C.sub }}>{v.products.length}</div>
              </div>
            ))}
          </div>
        </>)}

        {tab === "cobro" && (<>
          <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>⚙️ Configuración de Cobro</div>
          <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, padding: 26, maxWidth: 480 }}>
            <p style={{ fontSize: 13, color: C.sub, margin: "0 0 18px" }}>Esta información se muestra a los vendedores al pagar su membresía.</p>
            <Inp label="Número de Tarjeta (CUP/MLC)" value={bankInfo.card} onChange={v => setBankInfo(b => ({ ...b, card: v }))} placeholder="9225 XXXX XXXX XXXX" />
            <Inp label="Nombre del Titular" value={bankInfo.name} onChange={v => setBankInfo(b => ({ ...b, name: v }))} />
            <Inp label="Banco" value={bankInfo.bank} onChange={v => setBankInfo(b => ({ ...b, bank: v }))} />
            <Btn color={C.green} full icon="💾" onClick={() => { Object.assign(STORE.bankInfo, bankInfo); setSaved(true); setTimeout(() => setSaved(false), 2000); }}>
              {saved ? "✅ Guardado" : "Guardar Cambios"}
            </Btn>
            <div style={{ marginTop: 16, background: C.bg, borderRadius: 10, padding: 14, border: `1px dashed ${C.accent}44` }}>
              <div style={{ fontSize: 11, color: C.muted, marginBottom: 6 }}>Vista previa para vendedores:</div>
              <div style={{ fontSize: 13, color: C.sub, lineHeight: 2 }}>💳 <strong style={{ color: C.text }}>{bankInfo.card}</strong><br />👤 {bankInfo.name}<br />🏦 {bankInfo.bank}</div>
            </div>
          </div>
        </>)}

        {tab === "web" && (<>
          <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 18 }}>
            <div style={{ fontSize: 18, fontWeight: 800, color: C.text }}>🌐 Editor de Contenido Web</div>
            <div style={{ display: "flex", gap: 10, alignItems: "center" }}>
              {saved && <span style={{ fontSize: 12, color: C.green }}>✅ Publicado</span>}
              {!showAdd && <Btn small icon="➕" onClick={() => { setNb({ type: "banner", title: "", subtitle: "", content: "", link: "", color: C.accent, priority: "normal" }); setEditId(null); setShowAdd(true); }}>Agregar Bloque</Btn>}
            </div>
          </div>

          <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 12, padding: 14, marginBottom: 18, fontSize: 13, color: C.sub }}>
            📝 Agrega <strong style={{ color: C.accent }}>cualquier contenido</strong> a la web: banners, promociones, avisos, HTML, y más. Sin límites.
          </div>

          {showAdd && (
            <div style={{ background: C.card, border: `2px solid ${C.accent}44`, borderRadius: 16, padding: 24, marginBottom: 20 }}>
              <div style={{ fontSize: 15, fontWeight: 700, color: C.text, marginBottom: 16 }}>{editId ? "✏️ Editar Bloque" : "➕ Nuevo Bloque"}</div>
              <div style={{ marginBottom: 14 }}>
                <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 5, fontWeight: 600 }}>Tipo de Bloque</label>
                <select value={nb.type} onChange={e => setNb(b => ({ ...b, type: e.target.value }))}
                  style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "11px 14px", color: C.text, fontSize: 14, outline: "none" }}>
                  {BLOCK_TYPES.map(t => <option key={t.value} value={t.value}>{t.label}</option>)}
                </select>
              </div>
              <Inp label="Título *" value={nb.title} onChange={v => setNb(b => ({ ...b, title: v }))} placeholder="Ej: ¡Gran Oferta de Abril!" />
              <Inp label="Subtítulo" value={nb.subtitle} onChange={v => setNb(b => ({ ...b, subtitle: v }))} placeholder="Descripción corta" />
              <Textarea label="Contenido completo (opcional)" value={nb.content} onChange={v => setNb(b => ({ ...b, content: v }))} placeholder={nb.type === "html" ? "<div>Tu HTML aquí...</div>" : "Texto completo del bloque..."} rows={nb.type === "html" ? 5 : 3} />
              <Inp label="URL / Enlace (opcional)" value={nb.link} onChange={v => setNb(b => ({ ...b, link: v }))} placeholder="https://..." />
              <div style={{ marginBottom: 14 }}>
                <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 8, fontWeight: 600 }}>Prioridad</label>
                <div style={{ display: "flex", gap: 8 }}>
                  {["normal", "alta", "urgente"].map(p => (
                    <div key={p} onClick={() => setNb(b => ({ ...b, priority: p }))} style={{ padding: "6px 14px", borderRadius: 8, cursor: "pointer", background: nb.priority === p ? (p === "urgente" ? C.red : p === "alta" ? C.gold : C.accent) + "33" : C.bg, border: `1.5px solid ${nb.priority === p ? (p === "urgente" ? C.red : p === "alta" ? C.gold : C.accent) : C.border}`, color: nb.priority === p ? C.text : C.muted, fontSize: 13, fontWeight: 600 }}>
                      {p === "normal" ? "📄 Normal" : p === "alta" ? "⭐ Alta" : "🔴 Urgente"}
                    </div>
                  ))}
                </div>
              </div>
              <div style={{ marginBottom: 18 }}>
                <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 8, fontWeight: 600 }}>Color</label>
                <div style={{ display: "flex", gap: 8, flexWrap: "wrap", alignItems: "center" }}>
                  {PALETTE.map(pc => <div key={pc} onClick={() => setNb(b => ({ ...b, color: pc }))} style={{ width: 26, height: 26, borderRadius: "50%", background: pc, cursor: "pointer", border: nb.color === pc ? "3px solid #fff" : "3px solid transparent", transition: "border .12s" }} />)}
                  <input type="color" value={nb.color} onChange={e => setNb(b => ({ ...b, color: e.target.value }))} style={{ width: 26, height: 26, borderRadius: "50%", border: "none", cursor: "pointer", padding: 0 }} />
                </div>
              </div>
              <div style={{ display: "flex", gap: 10 }}>
                <Btn color={C.green} onClick={submitBlock} icon="✅">{editId ? "Actualizar" : "Publicar"}</Btn>
                <Btn ghost color={C.muted} onClick={() => { setShowAdd(false); setEditId(null); }}>Cancelar</Btn>
              </div>
            </div>
          )}

          <div style={{ display: "flex", flexDirection: "column", gap: 10 }}>
            {webBlocks.map((block, idx) => (
              <div key={block.id} style={{ background: C.card, border: `1px solid ${block.active ? block.color + "55" : C.border + "44"}`, borderLeft: `4px solid ${block.active ? block.color : C.muted}`, borderRadius: 12, padding: "13px 16px", display: "flex", alignItems: "flex-start", gap: 12, opacity: block.active ? 1 : 0.5, transition: "all .2s" }}>
                <div style={{ fontSize: 20, flexShrink: 0, marginTop: 2 }}>{BLOCK_ICONS[block.type] || "📝"}</div>
                <div style={{ flex: 1, minWidth: 0 }}>
                  <div style={{ display: "flex", alignItems: "center", gap: 8, flexWrap: "wrap", marginBottom: 3 }}>
                    <span style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{block.title}</span>
                    <Badge label={block.type} color={block.color} />
                    {block.priority !== "normal" && <Badge label={block.priority === "urgente" ? "🔴 Urgente" : "⭐ Alta"} color={block.priority === "urgente" ? C.red : C.gold} />}
                  </div>
                  {block.subtitle && <div style={{ fontSize: 12, color: C.sub }}>{block.subtitle}</div>}
                </div>
                <div style={{ display: "flex", gap: 5, flexShrink: 0, alignItems: "center" }}>
                  <button onClick={() => moveBlock(block.id, -1)} disabled={idx === 0} style={{ background: "none", border: "none", color: C.sub, cursor: "pointer", fontSize: 13 }}>▲</button>
                  <button onClick={() => moveBlock(block.id, 1)} disabled={idx === webBlocks.length - 1} style={{ background: "none", border: "none", color: C.sub, cursor: "pointer", fontSize: 13 }}>▼</button>
                  <Btn small ghost color={C.accent} onClick={() => startEdit(block)}>✏️</Btn>
                  <Btn small ghost color={block.active ? C.gold : C.green} onClick={() => toggleBlock(block.id)}>{block.active ? "⏸" : "▶"}</Btn>
                  <Btn small ghost color={C.red} onClick={() => removeBlock(block.id)}>🗑</Btn>
                </div>
              </div>
            ))}
            {webBlocks.length === 0 && <div style={{ textAlign: "center", padding: 44, color: C.muted, background: C.card, borderRadius: 14, border: `1px dashed ${C.border}` }}>🌐 No hay bloques. ¡Agrega el primero!</div>}
          </div>
        </>)}
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════
// VENDOR PANEL
// ═══════════════════════════════════════════════════════════════════════════
function VendorPanel({ vendor: initialVendor, onLogout }) {
  const [vendor, setVendor] = useState(initialVendor);
  const [tab, setTab] = useState("catalogo");
  const [showAdd, setShowAdd] = useState(false);
  const [trialBanner, setTrialBanner] = useState(vendor.status === "prueba");
  const [newProd, setNewProd] = useState({ name: "", price: "", priceNum: 0, category: "Tecnología", province: vendor.province, stock: "", commission: 0, description: "" });
  const [saved, setSaved] = useState(false);

  const syncVendor = () => {
    const v = STORE.vendors.find(x => x.id === vendor.id);
    if (v) setVendor({ ...v });
  };

  const tabs = [
    { id: "catalogo", icon: "🛍️", label: "Mi Catálogo" },
    { id: "plan", icon: "💳", label: "Mi Plan" },
    { id: "afiliados", icon: "🤝", label: "Afiliados" },
    { id: "perfil", icon: "🏪", label: "Mi Perfil" },
  ];

  const addProduct = () => {
    if (!newProd.name || !newProd.price) return;
    const p = { id: "p" + Date.now(), name: newProd.name, price: parseFloat(newProd.price.replace(/,/g, "")), priceStr: parseFloat(newProd.price).toLocaleString(), category: newProd.category, province: newProd.province, stock: parseInt(newProd.stock) || 1, photos: 0, views: 0, clicks: 0, commission: newProd.commission, description: newProd.description };
    const v = STORE.vendors.find(x => x.id === vendor.id);
    if (v) v.products.push(p);
    syncVendor();
    setNewProd({ name: "", price: "", priceNum: 0, category: "Tecnología", province: vendor.province, stock: "", commission: 0, description: "" });
    setShowAdd(false);
  };

  const deleteProduct = pid => {
    const v = STORE.vendors.find(x => x.id === vendor.id);
    if (v) v.products = v.products.filter(p => p.id !== pid);
    syncVendor();
  };

  const isTrial = vendor.status === "prueba";
  const trialLeft = isTrial && vendor.trialEnd ? Math.max(0, Math.floor((vendor.trialEnd - Date.now()) / 1000)) : 0;

  const maxProducts = vendor.plan === "semanal" ? 10 : vendor.plan === "quincenal" ? 30 : Infinity;
  const atLimit = isTrial && vendor.products.length >= 3;

  return (
    <div style={{ display: "flex", height: "100%", overflow: "hidden" }}>
      <Sidebar tabs={tabs} active={tab} onSelect={setTab} roleLabel="🔵 Vendedor" color={C.accent} userName={vendor.name} onLogout={onLogout} />
      <div style={{ flex: 1, display: "flex", overflow: "hidden" }}>
        <div style={{ flex: 1, padding: 26, overflowY: "auto" }}>

          {trialBanner && isTrial && (
            <div style={{ background: C.gold + "18", border: `1px solid ${C.gold}44`, borderRadius: 12, padding: "12px 16px", marginBottom: 18, display: "flex", justifyContent: "space-between", alignItems: "center" }}>
              <div style={{ fontSize: 13, color: C.gold }}>⚠️ <strong>Modo Prueba</strong> — Tu tienda tiene acceso limitado. Activa un plan para desbloquear todo.</div>
              <div style={{ display: "flex", gap: 8 }}>
                <Btn small color={C.gold} onClick={() => setTab("plan")}>🚀 Ver Planes</Btn>
                <Btn small ghost color={C.muted} onClick={() => setTrialBanner(false)}>✕</Btn>
              </div>
            </div>
          )}

          {tab === "catalogo" && (<>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 18 }}>
              <div style={{ fontSize: 18, fontWeight: 800, color: C.text }}>🛍️ Mi Catálogo</div>
              <Btn small icon="➕" onClick={() => { if (atLimit) return; setShowAdd(!showAdd); }} color={atLimit ? C.muted : C.accent} disabled={atLimit}>
                {atLimit ? "Límite alcanzado" : "Nuevo Producto"}
              </Btn>
            </div>
            {atLimit && (
              <div style={{ background: C.red + "15", border: `1px solid ${C.red}44`, borderRadius: 12, padding: "12px 16px", marginBottom: 18, fontSize: 13, color: C.red }}>
                🔒 Llegaste al límite de la prueba (3 productos). <span style={{ color: C.gold, cursor: "pointer", fontWeight: 700 }} onClick={() => setTab("plan")}>Activa un Plan →</span>
              </div>
            )}
            <div style={{ display: "flex", gap: 12, flexWrap: "wrap", marginBottom: 18 }}>
              <StatCard icon="📦" label="Productos" value={vendor.products.length} sub={`de ${maxProducts === Infinity ? "∞" : maxProducts} (${vendor.plan})`} color={C.accent} />
              <StatCard icon="👁️" label="Vistas Total" value={vendor.products.reduce((s, p) => s + p.views, 0)} color={C.purple} />
              <StatCard icon="🖱️" label="Clics WA" value={vendor.products.reduce((s, p) => s + p.clicks, 0)} color={C.green} />
            </div>

            {showAdd && (
              <div style={{ background: C.card, border: `1.5px solid ${C.accent}44`, borderRadius: 14, padding: 22, marginBottom: 18 }}>
                <div style={{ fontSize: 14, fontWeight: 700, color: C.text, marginBottom: 14 }}>➕ Nuevo Producto</div>
                <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10 }}>
                  <Inp label="Nombre del Producto" value={newProd.name} onChange={v => setNewProd(p => ({ ...p, name: v }))} placeholder="Ej: iPhone 14 Pro" />
                  <Inp label="Precio (CUP)" value={newProd.price} onChange={v => setNewProd(p => ({ ...p, price: v }))} placeholder="75000" />
                  <div>
                    <label style={{ fontSize: 12, color: C.sub, display: "block", marginBottom: 5, fontWeight: 600 }}>Categoría</label>
                    <select value={newProd.category} onChange={e => setNewProd(p => ({ ...p, category: e.target.value }))}
                      style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "11px 14px", color: C.text, fontSize: 14, outline: "none" }}>
                      {CATEGORIES.slice(1).map(c => <option key={c}>{c}</option>)}
                    </select>
                  </div>
                  <Inp label="Stock disponible" value={newProd.stock} onChange={v => setNewProd(p => ({ ...p, stock: v }))} placeholder="1" />
                </div>
                <Textarea label="Descripción" value={newProd.description} onChange={v => setNewProd(p => ({ ...p, description: v }))} placeholder="Describe tu producto: estado, características, detalles..." />
                <div style={{ marginBottom: 16 }}>
                  <label style={{ fontSize: 12, color: C.sub, fontWeight: 600, display: "block", marginBottom: 5 }}>Comisión para Afiliados: <span style={{ color: C.gold }}>{newProd.commission}%</span></label>
                  <input type="range" min={0} max={20} value={newProd.commission} onChange={e => setNewProd(p => ({ ...p, commission: Number(e.target.value) }))} style={{ width: "100%", accentColor: C.gold }} />
                  <div style={{ display: "flex", justifyContent: "space-between", fontSize: 11, color: C.muted }}><span>Sin comisión</span><span>20% máx</span></div>
                </div>
                <div style={{ display: "flex", gap: 10 }}>
                  <Btn color={C.accent} onClick={addProduct} icon="✅">Publicar Producto</Btn>
                  <Btn ghost color={C.muted} onClick={() => setShowAdd(false)}>Cancelar</Btn>
                </div>
              </div>
            )}

            <div style={{ display: "flex", flexDirection: "column", gap: 10 }}>
              {vendor.products.length === 0 && <div style={{ textAlign: "center", padding: 40, color: C.muted, background: C.card, borderRadius: 14, border: `1px dashed ${C.border}` }}>📦 No tienes productos aún. ¡Agrega el primero!</div>}
              {vendor.products.map(p => (
                <div key={p.id} style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 12, padding: "14px 18px" }}>
                  <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                    <div style={{ width: 44, height: 44, background: C.bg, borderRadius: 10, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 20 }}>📦</div>
                    <div style={{ flex: 1 }}>
                      <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{p.name}</div>
                      <div style={{ fontSize: 12, color: C.muted }}>{p.category} · Stock: {p.stock} · {p.province}</div>
                    </div>
                    <div style={{ textAlign: "right", display: "flex", alignItems: "center", gap: 10 }}>
                      <div>
                        <div style={{ fontSize: 15, fontWeight: 800, color: C.green }}>${p.priceStr || p.price.toLocaleString()} CUP</div>
                        {p.commission > 0 && <Badge label={`${p.commission}% af.`} color={C.gold} />}
                      </div>
                      <Btn small ghost color={C.red} onClick={() => deleteProduct(p.id)}>🗑</Btn>
                    </div>
                  </div>
                  <div style={{ display: "flex", gap: 12, marginTop: 10, paddingTop: 10, borderTop: `1px solid ${C.border}30` }}>
                    <span style={{ fontSize: 12, color: C.muted }}>👁️ {p.views} vistas</span>
                    <span style={{ fontSize: 12, color: C.muted }}>🖱️ {p.clicks} clics</span>
                    {p.description && <span style={{ fontSize: 12, color: C.muted, overflow: "hidden", textOverflow: "ellipsis", whiteSpace: "nowrap" }}>📝 {p.description.slice(0, 40)}...</span>}
                  </div>
                </div>
              ))}
            </div>
          </>)}

          {tab === "plan" && (<>
            <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>💳 Planes de Membresía</div>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(3, 1fr)", gap: 14, marginBottom: 22 }}>
              {[{ id: "semanal", name: "Semanal", price: "$500 CUP", color: C.sub, p: 10, f: 3, af: "No", b: "Sello Básico" }, { id: "quincenal", name: "Quincenal", price: "$1,000 CUP", color: C.accent, p: 30, f: 6, af: "5 productos", b: "Sello Destacado" }, { id: "mensual", name: "Mensual", price: "$2,000 CUP", color: C.gold, p: "∞", f: 12, af: "Catálogo completo", b: "Verificado ✅" }].map(plan => (
                <div key={plan.id} style={{ background: C.card, border: `2px solid ${vendor.plan === plan.id ? plan.color : C.border}`, borderRadius: 16, padding: 20, position: "relative", boxShadow: vendor.plan === plan.id ? `0 0 24px ${plan.color}22` : "none" }}>
                  {vendor.plan === plan.id && <div style={{ position: "absolute", top: -11, left: "50%", transform: "translateX(-50%)", background: plan.color, color: "#000", fontSize: 9, fontWeight: 800, borderRadius: 20, padding: "2px 10px", whiteSpace: "nowrap" }}>PLAN ACTUAL</div>}
                  <div style={{ fontSize: 16, fontWeight: 800, color: plan.color }}>{plan.name}</div>
                  <div style={{ fontSize: 22, fontWeight: 900, color: C.text, margin: "8px 0 12px" }}>{plan.price}</div>
                  <div style={{ fontSize: 12, color: C.sub, lineHeight: 2 }}>📦 {plan.p} prods<br />📸 {plan.f} fotos<br />🤝 {plan.af}<br />🏷️ {plan.b}</div>
                  <div style={{ marginTop: 14 }}><Btn full color={plan.color} small>Renovar</Btn></div>
                </div>
              ))}
            </div>
            <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, padding: 20, maxWidth: 400 }}>
              <div style={{ fontSize: 13, fontWeight: 700, color: C.text, marginBottom: 10 }}>📲 Datos para el Pago</div>
              <div style={{ fontSize: 14, color: C.sub, lineHeight: 2 }}>💳 <strong style={{ color: C.text }}>{STORE.bankInfo.card}</strong><br />👤 {STORE.bankInfo.name}<br />🏦 {STORE.bankInfo.bank}</div>
              <div style={{ marginTop: 10, fontSize: 12, color: C.muted }}>Envía la captura de pantalla del pago al soporte y será activado de inmediato.</div>
            </div>
          </>)}

          {tab === "afiliados" && (<>
            <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 8 }}>🤝 Módulo de Afiliados</div>
            <p style={{ fontSize: 13, color: C.sub, margin: "0 0 18px" }}>Los promotores que compartan tu link ganarán la comisión que tú defines.</p>
            {vendor.products.filter(p => p.commission > 0).length === 0
              ? <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 12, padding: 30, textAlign: "center", color: C.muted }}>🤝 Ningún producto tiene comisión activa. Edita un producto y define el % de comisión.</div>
              : vendor.products.filter(p => p.commission > 0).map(p => (
                <div key={p.id} style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 12, padding: "14px 18px", marginBottom: 10, display: "flex", alignItems: "center", gap: 14 }}>
                  <div style={{ flex: 1 }}>
                    <div style={{ fontSize: 14, fontWeight: 600, color: C.text }}>{p.name}</div>
                    <div style={{ fontSize: 12, color: C.muted }}>${(p.priceStr || p.price)} CUP</div>
                  </div>
                  <div style={{ fontSize: 22, fontWeight: 900, color: C.gold }}>{p.commission}%</div>
                  <Badge label="Comisión Activa" color={C.gold} />
                </div>
              ))
            }
          </>)}

          {tab === "perfil" && (<>
            <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>🏪 Mi Perfil Público</div>
            <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 16, padding: 26, maxWidth: 460 }}>
              <div style={{ display: "flex", gap: 14, marginBottom: 22, alignItems: "center" }}>
                <div style={{ width: 56, height: 56, background: C.accent + "22", borderRadius: "50%", display: "flex", alignItems: "center", justifyContent: "center", fontSize: 26 }}>🏪</div>
                <div>
                  <div style={{ fontSize: 18, fontWeight: 800, color: C.text }}>{vendor.name}</div>
                  <div style={{ fontSize: 13, color: C.muted, marginBottom: 4 }}>{vendor.province}</div>
                  <Badge label={vendor.plan === "mensual" ? "Vendedor Verificado ✅" : vendor.plan === "quincenal" ? "Sello Destacado" : "Sello Básico"} color={vendor.plan === "mensual" ? C.gold : C.accent} />
                </div>
              </div>
              <Inp label="Nombre de la Tienda" value={vendor.name} onChange={() => {}} />
              <Sel label="Provincia" value={vendor.province} onChange={() => {}} options={PROVINCES.slice(1)} />
              <Inp label="WhatsApp de Contacto" value={vendor.whatsapp} onChange={() => {}} />
              <Inp label="Correo Electrónico" value={vendor.email} onChange={() => {}} />
              <Btn color={C.accent} full icon="💾" onClick={() => { setSaved(true); setTimeout(() => setSaved(false), 2000); }}>
                {saved ? "✅ Guardado" : "Guardar Cambios"}
              </Btn>
            </div>
          </>)}
        </div>

        {isTrial && trialLeft > 0 && (
          <div style={{ width: 230, padding: 16, borderLeft: `1px solid ${C.border}`, flexShrink: 0, overflowY: "auto" }}>
            <Countdown seconds={trialLeft} />
          </div>
        )}
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════
// AFFILIATE PANEL
// ═══════════════════════════════════════════════════════════════════════════
function AffiliatePanel({ affiliate, onLogout }) {
  const [copied, setCopied] = useState(null);
  const copy = id => { setCopied(id); setTimeout(() => setCopied(null), 2000); };
  const allProds = getAllProducts().filter(p => p.commission > 0);

  const tabs = [
    { id: "market", icon: "🛍️", label: "Marketplace" },
    { id: "wallet", icon: "💰", label: "Mis Ganancias" },
  ];
  const [tab, setTab] = useState("market");

  return (
    <div style={{ display: "flex", height: "100%", overflow: "hidden" }}>
      <Sidebar tabs={tabs} active={tab} onSelect={setTab} roleLabel="🟢 Afiliado" color={C.green} userName={affiliate.name} onLogout={onLogout} />
      <div style={{ flex: 1, padding: 26, overflowY: "auto" }}>
        <div style={{ display: "flex", gap: 12, flexWrap: "wrap", marginBottom: 22 }}>
          <StatCard icon="💰" label="Ganado Total" value={`$${affiliate.earnings.toLocaleString()}`} sub="CUP" color={C.gold} />
          <StatCard icon="🛒" label="Ventas" value={affiliate.sales} sub="este mes" color={C.green} />
          <StatCard icon="🔗" label="Links Activos" value={affiliate.links} color={C.accent} />
        </div>

        {tab === "market" && (<>
          <div style={{ fontSize: 15, fontWeight: 700, color: C.text, marginBottom: 14 }}>🛍️ Productos con Comisión</div>
          {allProds.length === 0 && <div style={{ textAlign: "center", padding: 40, color: C.muted }}>No hay productos con comisión disponibles aún.</div>}
          <div style={{ display: "flex", flexDirection: "column", gap: 12 }}>
            {allProds.map(p => (
              <div key={p.id} style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, padding: 18, display: "flex", gap: 14, alignItems: "center" }}>
                <div style={{ width: 46, height: 46, background: C.bg, borderRadius: 10, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 20 }}>🏷️</div>
                <div style={{ flex: 1 }}>
                  <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{p.name}</div>
                  <div style={{ fontSize: 12, color: C.muted }}>por {p.vendorName}</div>
                  <div style={{ marginTop: 4 }}>
                    <span style={{ fontSize: 15, fontWeight: 800, color: C.green }}>${p.priceStr || p.price.toLocaleString()} CUP</span>
                    <span style={{ fontSize: 13, color: C.gold, marginLeft: 10 }}>🤝 {p.commission}% comisión</span>
                  </div>
                </div>
                <Btn small color={C.accent} onClick={() => copy(p.id)} icon="🔗">{copied === p.id ? "¡Copiado!" : "Mi Link"}</Btn>
              </div>
            ))}
          </div>
        </>)}

        {tab === "wallet" && (<>
          <div style={{ fontSize: 15, fontWeight: 700, color: C.text, marginBottom: 14 }}>💰 Mis Ganancias</div>
          <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, padding: 24, maxWidth: 400, textAlign: "center" }}>
            <div style={{ fontSize: 40, fontWeight: 900, color: C.gold, fontFamily: "monospace" }}>${affiliate.earnings.toLocaleString()}</div>
            <div style={{ fontSize: 14, color: C.muted, marginTop: 4 }}>CUP acumulados</div>
            <div style={{ marginTop: 16, fontSize: 13, color: C.sub }}>{affiliate.sales} ventas concretadas este mes</div>
            <div style={{ marginTop: 20 }}><Btn color={C.green} full>💸 Solicitar Pago</Btn></div>
          </div>
        </>)}
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════
// BUYER PANEL — advanced search
// ═══════════════════════════════════════════════════════════════════════════
function BuyerPanel({ buyer, onLogout }) {
  const [search, setSearch] = useState("");
  const [cat, setCat] = useState("Todas");
  const [province, setProvince] = useState("Todas");
  const [sortBy, setSortBy] = useState("relevancia");
  const [priceMin, setPriceMin] = useState("");
  const [priceMax, setPriceMax] = useState("");
  const [showFilters, setShowFilters] = useState(false);
  const [favorites, setFavorites] = useState(buyer.favorites || []);
  const [tab, setTab] = useState("buscar");

  const toggleFav = id => {
    const next = favorites.includes(id) ? favorites.filter(x => x !== id) : [...favorites, id];
    setFavorites(next);
    const b = STORE.buyers.find(x => x.id === buyer.id);
    if (b) b.favorites = next;
  };

  const allProducts = getAllProducts();

  const filtered = allProducts.filter(p => {
    const q = search.toLowerCase().trim();
    const matchSearch = !q || p.name.toLowerCase().includes(q) || p.category.toLowerCase().includes(q) || p.vendorName.toLowerCase().includes(q) || (p.description && p.description.toLowerCase().includes(q));
    const matchCat = cat === "Todas" || p.category === cat;
    const matchProv = province === "Todas" || p.vendorProvince === province || p.province === province;
    const matchMin = !priceMin || p.price >= parseFloat(priceMin);
    const matchMax = !priceMax || p.price <= parseFloat(priceMax);
    return matchSearch && matchCat && matchProv && matchMin && matchMax;
  }).sort((a, b) => {
    if (sortBy === "precio_asc") return a.price - b.price;
    if (sortBy === "precio_desc") return b.price - a.price;
    if (sortBy === "mas_vistos") return b.views - a.views;
    return 0;
  });

  const hasFilters = cat !== "Todas" || province !== "Todas" || priceMin || priceMax || sortBy !== "relevancia";
  const resetFilters = () => { setCat("Todas"); setProvince("Todas"); setPriceMin(""); setPriceMax(""); setSortBy("relevancia"); };

  const tabs = [
    { id: "buscar", icon: "🔍", label: "Buscar" },
    { id: "favoritos", icon: "❤️", label: `Favoritos (${favorites.length})` },
  ];

  return (
    <div style={{ display: "flex", height: "100%", overflow: "hidden" }}>
      <Sidebar tabs={tabs} active={tab} onSelect={setTab} roleLabel="🟡 Comprador" color={C.gold} userName={buyer.name} onLogout={onLogout} />
      <div style={{ flex: 1, padding: 26, overflowY: "auto" }}>

        {tab === "buscar" && (<>
          {/* Search bar */}
          <div style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, padding: 16, marginBottom: 14 }}>
            <div style={{ display: "flex", gap: 10, marginBottom: 12 }}>
              <div style={{ flex: 1, position: "relative" }}>
                <input value={search} onChange={e => setSearch(e.target.value)} placeholder="🔍 ¿Qué estás buscando? Ej: iPhone, nevera, ropa..."
                  style={{ width: "100%", background: C.bg, border: `1.5px solid ${search ? C.accent : C.border}`, borderRadius: 9, padding: "11px 14px 11px 14px", color: C.text, fontSize: 14, outline: "none", boxSizing: "border-box", transition: "border .15s" }} />
                {search && <button onClick={() => setSearch("")} style={{ position: "absolute", right: 10, top: "50%", transform: "translateY(-50%)", background: "none", border: "none", color: C.muted, cursor: "pointer", fontSize: 16 }}>✕</button>}
              </div>
              <Btn small color={showFilters ? C.accent : C.muted} ghost={!showFilters} onClick={() => setShowFilters(f => !f)} icon="⚙️">
                Filtros {hasFilters ? "●" : ""}
              </Btn>
            </div>

            {showFilters && (
              <div style={{ borderTop: `1px solid ${C.border}30`, paddingTop: 14 }}>
                <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 10, marginBottom: 10 }}>
                  <div>
                    <label style={{ fontSize: 11, color: C.muted, display: "block", marginBottom: 4, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1 }}>Categoría</label>
                    <select value={cat} onChange={e => setCat(e.target.value)} style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "9px 12px", color: C.text, fontSize: 13, outline: "none" }}>
                      {CATEGORIES.map(c => <option key={c}>{c}</option>)}
                    </select>
                  </div>
                  <div>
                    <label style={{ fontSize: 11, color: C.muted, display: "block", marginBottom: 4, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1 }}>Provincia</label>
                    <select value={province} onChange={e => setProvince(e.target.value)} style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "9px 12px", color: C.text, fontSize: 13, outline: "none" }}>
                      {PROVINCES.map(p => <option key={p}>{p}</option>)}
                    </select>
                  </div>
                  <div>
                    <label style={{ fontSize: 11, color: C.muted, display: "block", marginBottom: 4, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1 }}>Precio Mínimo (CUP)</label>
                    <input value={priceMin} onChange={e => setPriceMin(e.target.value)} placeholder="0" type="number"
                      style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "9px 12px", color: C.text, fontSize: 13, outline: "none", boxSizing: "border-box" }} />
                  </div>
                  <div>
                    <label style={{ fontSize: 11, color: C.muted, display: "block", marginBottom: 4, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1 }}>Precio Máximo (CUP)</label>
                    <input value={priceMax} onChange={e => setPriceMax(e.target.value)} placeholder="Sin límite" type="number"
                      style={{ width: "100%", background: C.bg, border: `1.5px solid ${C.border}`, borderRadius: 9, padding: "9px 12px", color: C.text, fontSize: 13, outline: "none", boxSizing: "border-box" }} />
                  </div>
                </div>
                <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between" }}>
                  <div>
                    <label style={{ fontSize: 11, color: C.muted, display: "block", marginBottom: 4, fontWeight: 600, textTransform: "uppercase", letterSpacing: 1 }}>Ordenar por</label>
                    <div style={{ display: "flex", gap: 8 }}>
                      {[["relevancia", "⭐ Relevancia"], ["mas_vistos", "👁️ Más vistos"], ["precio_asc", "💲 Menor precio"], ["precio_desc", "💲 Mayor precio"]].map(([val, lbl]) => (
                        <div key={val} onClick={() => setSortBy(val)} style={{ padding: "5px 12px", borderRadius: 7, cursor: "pointer", background: sortBy === val ? C.gold + "33" : C.bg, border: `1.5px solid ${sortBy === val ? C.gold : C.border}`, color: sortBy === val ? C.gold : C.muted, fontSize: 12, fontWeight: sortBy === val ? 700 : 400, whiteSpace: "nowrap" }}>{lbl}</div>
                      ))}
                    </div>
                  </div>
                  {hasFilters && <Btn small ghost color={C.red} onClick={resetFilters}>✕ Limpiar filtros</Btn>}
                </div>
              </div>
            )}
          </div>

          {/* Category pills */}
          <div style={{ display: "flex", gap: 8, marginBottom: 16, overflowX: "auto", paddingBottom: 4, flexWrap: "nowrap" }}>
            {CATEGORIES.map(c => (
              <div key={c} onClick={() => setCat(c)} style={{ padding: "5px 14px", borderRadius: 20, cursor: "pointer", background: cat === c ? C.gold + "33" : C.card, border: `1.5px solid ${cat === c ? C.gold : C.border}`, color: cat === c ? C.gold : C.sub, fontSize: 12, fontWeight: cat === c ? 700 : 400, whiteSpace: "nowrap", transition: "all .13s", flexShrink: 0 }}>{c}</div>
            ))}
          </div>

          {/* Results count */}
          <div style={{ fontSize: 12, color: C.muted, marginBottom: 14 }}>
            {search || hasFilters ? (
              <>{filtered.length} resultado{filtered.length !== 1 ? "s" : ""} {search && <>para "<strong style={{ color: C.text }}>{search}</strong>"</>}</>
            ) : (
              <>{allProducts.length} productos disponibles</>
            )}
          </div>

          {filtered.length === 0 && (
            <div style={{ textAlign: "center", padding: 50, color: C.muted, background: C.card, borderRadius: 14, border: `1px dashed ${C.border}` }}>
              <div style={{ fontSize: 36, marginBottom: 12 }}>🔍</div>
              <div style={{ fontSize: 15, fontWeight: 600, marginBottom: 6, color: C.sub }}>No se encontraron resultados</div>
              <div style={{ fontSize: 13 }}>Intenta con otra palabra clave o ajusta los filtros</div>
              {hasFilters && <div style={{ marginTop: 14 }}><Btn small ghost color={C.gold} onClick={resetFilters}>Limpiar filtros</Btn></div>}
            </div>
          )}

          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(220px, 1fr))", gap: 14 }}>
            {filtered.map(p => (
              <div key={p.id} style={{ background: C.card, border: `1px solid ${C.border}`, borderRadius: 14, overflow: "hidden", transition: "transform .15s, border .15s" }}
                onMouseEnter={e => { e.currentTarget.style.transform = "translateY(-2px)"; e.currentTarget.style.borderColor = C.accent + "66"; }}
                onMouseLeave={e => { e.currentTarget.style.transform = "none"; e.currentTarget.style.borderColor = C.border; }}>
                <div style={{ height: 110, background: `linear-gradient(135deg, ${C.bg}, ${C.surface})`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 38, position: "relative" }}>
                  📦
                  <button onClick={() => toggleFav(p.id)} style={{ position: "absolute", top: 8, right: 8, background: C.surface + "cc", border: "none", borderRadius: "50%", width: 30, height: 30, cursor: "pointer", fontSize: 13, display: "flex", alignItems: "center", justifyContent: "center" }}>
                    {favorites.includes(p.id) ? "❤️" : "🤍"}
                  </button>
                  {p.commission > 0 && <div style={{ position: "absolute", top: 8, left: 8, background: C.gold + "cc", color: "#000", fontSize: 10, fontWeight: 800, borderRadius: 5, padding: "2px 6px" }}>🤝 AF</div>}
                </div>
                <div style={{ padding: "12px 14px" }}>
                  <div style={{ fontSize: 13, fontWeight: 700, color: C.text, marginBottom: 2 }}>{p.name}</div>
                  <div style={{ fontSize: 11, color: C.muted, marginBottom: 6 }}>{p.category} · 📍 {p.vendorProvince || p.province}</div>
                  {p.description && <div style={{ fontSize: 11, color: C.sub, marginBottom: 8, lineHeight: 1.4, overflow: "hidden", display: "-webkit-box", WebkitLineClamp: 2, WebkitBoxOrient: "vertical" }}>{p.description}</div>}
                  <div style={{ fontSize: 18, fontWeight: 900, color: C.green, marginBottom: 8 }}>${p.priceStr || p.price.toLocaleString()} <span style={{ fontSize: 10, color: C.muted }}>CUP</span></div>
                  <div style={{ fontSize: 11, color: C.muted, marginBottom: 8 }}>🏪 {p.vendorName} · 👁 {p.views}</div>
                  <a href={`https://wa.me/${(p.vendorWA || "5353215857").replace(/\D/g, "")}`} target="_blank" rel="noopener noreferrer" style={{ textDecoration: "none", display: "block", background: "#25d366", color: "#fff", borderRadius: 8, padding: "8px", textAlign: "center", fontWeight: 700, fontSize: 12 }}>
                    💬 Contactar por WhatsApp
                  </a>
                </div>
              </div>
            ))}
          </div>
        </>)}

        {tab === "favoritos" && (<>
          <div style={{ fontSize: 18, fontWeight: 800, color: C.text, marginBottom: 18 }}>❤️ Mis Favoritos</div>
          {favorites.length === 0
            ? <div style={{ textAlign: "center", padding: 50, color: C.muted, background: C.card, borderRadius: 14, border: `1px dashed ${C.border}` }}>❤️ No tienes favoritos guardados aún.<br /><span style={{ fontSize: 12 }}>Toca el corazón en cualquier producto.</span></div>
            : (
              <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(220px, 1fr))", gap: 14 }}>
                {allProducts.filter(p => favorites.includes(p.id)).map(p => (
                  <div key={p.id} style={{ background: C.card, border: `1px solid ${C.red}44`, borderRadius: 14, overflow: "hidden" }}>
                    <div style={{ height: 100, background: `linear-gradient(135deg, ${C.bg}, ${C.surface})`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 34, position: "relative" }}>
                      📦
                      <button onClick={() => toggleFav(p.id)} style={{ position: "absolute", top: 8, right: 8, background: C.surface + "cc", border: "none", borderRadius: "50%", width: 28, height: 28, cursor: "pointer", fontSize: 12, display: "flex", alignItems: "center", justifyContent: "center" }}>❤️</button>
                    </div>
                    <div style={{ padding: "12px 14px" }}>
                      <div style={{ fontSize: 13, fontWeight: 700, color: C.text, marginBottom: 2 }}>{p.name}</div>
                      <div style={{ fontSize: 11, color: C.muted, marginBottom: 6 }}>{p.vendorName}</div>
                      <div style={{ fontSize: 17, fontWeight: 900, color: C.green, marginBottom: 8 }}>${p.priceStr || p.price.toLocaleString()} CUP</div>
                      <a href={`https://wa.me/${(p.vendorWA || "5353215857").replace(/\D/g, "")}`} target="_blank" rel="noopener noreferrer" style={{ textDecoration: "none", display: "block", background: "#25d366", color: "#fff", borderRadius: 8, padding: "7px", textAlign: "center", fontWeight: 700, fontSize: 12 }}>
                        💬 WhatsApp
                      </a>
                    </div>
                  </div>
                ))}
              </div>
            )
          }
        </>)}
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════
// HOME — Role selector
// ═══════════════════════════════════════════════════════════════════════════
function Home({ onSelect }) {
  const roles = [
    { id: "admin", label: "Administrador", icon: "🔴", color: C.red, desc: "Control total de la plataforma" },
    { id: "vendor", label: "Vendedor", icon: "🔵", color: C.accent, desc: "Gestiona tu tienda y productos" },
    { id: "affiliate", label: "Afiliado / Promotor", icon: "🟢", color: C.green, desc: "Gana comisiones promoviendo" },
    { id: "buyer", label: "Comprador", icon: "🟡", color: C.gold, desc: "Explora y compra productos" },
  ];

  return (
    <div style={{ minHeight: "100vh", background: C.bg, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", padding: 24, fontFamily: "'Segoe UI', sans-serif" }}>
      <style>{`* { box-sizing: border-box; } ::-webkit-scrollbar { width: 5px } ::-webkit-scrollbar-track { background: ${C.bg} } ::-webkit-scrollbar-thumb { background: ${C.border}; border-radius: 3px }`}</style>
      <div style={{ textAlign: "center", marginBottom: 40 }}>
        <div style={{ fontSize: 52, marginBottom: 10 }}>🌐</div>
        <div style={{ fontSize: 32, fontWeight: 900, fontFamily: "monospace", color: C.text, letterSpacing: -1 }}>Nexo<span style={{ color: C.accent }}>Mercado</span></div>
        <div style={{ fontSize: 14, color: C.muted, marginTop: 8 }}>El marketplace profesional de Cuba 🇨🇺</div>
        <div style={{ fontSize: 12, color: C.sub, marginTop: 5 }}>Cada usuario tiene su propio espacio independiente</div>
      </div>

      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 14, width: "100%", maxWidth: 500 }}>
        {roles.map(r => (
          <div key={r.id} onClick={() => onSelect(r.id)}
            style={{ background: C.card, border: `1.5px solid ${C.border}`, borderRadius: 16, padding: "22px 16px", cursor: "pointer", textAlign: "center", transition: "all .18s" }}
            onMouseEnter={e => { e.currentTarget.style.borderColor = r.color; e.currentTarget.style.transform = "translateY(-3px)"; e.currentTarget.style.background = r.color + "10"; }}
            onMouseLeave={e => { e.currentTarget.style.borderColor = C.border; e.currentTarget.style.transform = "none"; e.currentTarget.style.background = C.card; }}>
            <div style={{ fontSize: 30, marginBottom: 8 }}>{r.icon}</div>
            <div style={{ fontSize: 14, fontWeight: 700, color: C.text }}>{r.label}</div>
            <div style={{ fontSize: 11, color: C.muted, marginTop: 4 }}>{r.desc}</div>
          </div>
        ))}
      </div>

      <div style={{ marginTop: 28, textAlign: "center", fontSize: 12, color: C.muted }}>
        💬 <a href="https://wa.me/5353215857" style={{ color: C.accent, textDecoration: "none" }}>+53 53215857</a>
        {" · "}
        <a href="mailto:maikolgb07@gmail.com" style={{ color: C.accent, textDecoration: "none" }}>maikolgb07@gmail.com</a>
      </div>
    </div>
  );
}

// ═══════════════════════════════════════════════════════════════════════════
// MAIN APP
// ═══════════════════════════════════════════════════════════════════════════
export default function NexoMercado() {
  const [screen, setScreen] = useState("home"); // home | admin_login | vendor_auth | affiliate_auth | buyer_auth | admin | vendor | affiliate | buyer
  const [vendorUser, setVendorUser] = useState(null);
  const [affiliateUser, setAffiliateUser] = useState(null);
  const [buyerUser, setBuyerUser] = useState(null);

  const go = s => setScreen(s);
  const logout = () => { setVendorUser(null); setAffiliateUser(null); setBuyerUser(null); go("home"); };

  // Home → auth screens
  if (screen === "home") return <Home onSelect={role => go(role + (role === "admin" ? "_login" : "_auth"))} />;

  const shell = (content, showBack = false) => (
    <div style={{ height: "100vh", background: C.bg, display: "flex", flexDirection: "column", fontFamily: "'Segoe UI', sans-serif", color: C.text }}>
      <style>{`* { box-sizing: border-box; } ::-webkit-scrollbar { width: 5px } ::-webkit-scrollbar-track { background: ${C.bg} } ::-webkit-scrollbar-thumb { background: ${C.border}; border-radius: 3px }`}</style>
      <Topbar onBack={showBack ? () => go("home") : null} />
      <div style={{ flex: 1, display: "flex", overflow: "hidden" }}>{content}</div>
    </div>
  );

  if (screen === "admin_login") return <AdminLogin onSuccess={() => go("admin")} />;
  if (screen === "vendor_auth") return <VendorAuth onSuccess={v => { setVendorUser(v); go("vendor"); }} />;
  if (screen === "affiliate_auth") return <AffiliateAuth onSuccess={a => { setAffiliateUser(a); go("affiliate"); }} />;
  if (screen === "buyer_auth") return <BuyerAuth onSuccess={b => { setBuyerUser(b); go("buyer"); }} />;

  if (screen === "admin") return shell(<AdminPanel onLogout={logout} />, false);
  if (screen === "vendor" && vendorUser) return shell(<VendorPanel vendor={vendorUser} onLogout={logout} />, false);
  if (screen === "affiliate" && affiliateUser) return shell(<AffiliatePanel affiliate={affiliateUser} onLogout={logout} />, false);
  if (screen === "buyer" && buyerUser) return shell(<BuyerPanel buyer={buyerUser} onLogout={logout} />, false);

  return <Home onSelect={role => go(role + (role === "admin" ? "_login" : "_auth"))} />;
}
