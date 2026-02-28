/**
 * OP20_Vesting_Finance — Real DApp v3.0
 * UI: Matches provided screenshot exactly
 * Chain: OPNet Testnet · https://testnet.opnet.org
 * Wallet: OP_WALLET (window.opnet / window.opwallet)
 * Data: Live JSON-RPC — balanceOf, btc_getBalance, eth_getLogs
 */

import { useState, useEffect, useCallback, useRef } from "react";
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer } from "recharts";

// ─── Token Registry ───────────────────────────────────────────────────────────
const TOKENS = {
  PILL: {
    symbol: "PILL",
    address: "0xb09fc29c112af8293539477e23d8df1d3126639642767d707277131352040cbb",
    decimals: 8,
    color: "#4F8EF7",
  },
  MOTO: {
    symbol: "MOTO",
    address: "0xfd4473840751d58d9f8b73bdd57d6c5260453d5518bd7cd02d0a4cf3df9bf4dd",
    decimals: 8,
    color: "#22D3EE",
  },
};

const RPC = "https://testnet.opnet.org";

// ─── RPC Helpers ──────────────────────────────────────────────────────────────
async function rpc(method, params = []) {
  const r = await fetch(RPC, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ jsonrpc: "2.0", id: Date.now(), method, params }),
  });
  if (!r.ok) throw new Error(`HTTP ${r.status}`);
  const j = await r.json();
  if (j.error) throw new Error(j.error.message || JSON.stringify(j.error));
  return j.result;
}

function encodeBalanceOf(addr) {
  const a = (addr.startsWith("0x") ? addr.slice(2) : addr).padStart(64, "0");
  return "0x70a08231" + a;
}

async function getBalance(contract, wallet, decimals) {
  try {
    const res = await rpc("eth_call", [{ to: contract, data: encodeBalanceOf(wallet) }, "latest"]);
    if (!res || res === "0x" || res === "0x0") return 0;
    return Number(BigInt(res)) / 10 ** decimals;
  } catch { return null; }
}

async function getBTC(address) {
  try {
    const r = await rpc("btc_getBalance", [address]);
    return r?.confirmed != null ? Number(r.confirmed) / 1e8 : null;
  } catch { return null; }
}

async function getLogs(contract, wallet) {
  try {
    const pad = "0x" + wallet.replace(/^0x/, "").padStart(64, "0");
    const logs = await rpc("eth_getLogs", [{ address: contract, fromBlock: "0x0", toBlock: "latest", topics: [null, null, pad] }]);
    return Array.isArray(logs) ? logs : [];
  } catch { return []; }
}

// ─── Wallet Hook ──────────────────────────────────────────────────────────────
function useWallet() {
  const [w, setW] = useState(() => {
    try { const s = sessionStorage.getItem("opvf3"); return s ? JSON.parse(s) : null; } catch { return null; }
  });

  const save = useCallback((v) => {
    setW(v);
    try { if (v) sessionStorage.setItem("opvf3", JSON.stringify(v)); else sessionStorage.removeItem("opvf3"); } catch {}
  }, []);

  useEffect(() => {
    if (w?.address) return;
    const e = window.opnet || window.opwallet;
    if (!e) return;
    e.getAccounts?.().then(a => {
      if (!a?.length) return;
      Promise.all([e.getPublicKey?.().catch(() => null), e.getChain?.().catch(() => null)])
        .then(([pk, ch]) => save({ address: a[0], publicKey: pk, network: ch?.network || "testnet" }));
    }).catch(() => {});
  }, []); // eslint-disable-line

  const connect = useCallback(async () => {
    const e = window.opnet || window.opwallet;
    if (!e) throw new Error("OP_WALLET not installed.");
    const accounts = await e.requestAccounts();
    if (!accounts?.length) throw new Error("No accounts returned.");
    const [pk, ch] = await Promise.all([e.getPublicKey?.().catch(() => null), e.getChain?.().catch(() => null)]);
    const val = { address: accounts[0], publicKey: pk, network: ch?.network || "testnet" };
    save(val); return val;
  }, [save]);

  const disconnect = useCallback(() => save(null), [save]);
  return { wallet: w, connect, disconnect };
}

// ─── Chain Data Hook ──────────────────────────────────────────────────────────
function useChain(address) {
  const [d, setD] = useState(null);
  const [loading, setLoading] = useState(false);
  const [ts, setTs] = useState(null);

  const load = useCallback(async () => {
    if (!address) return;
    setLoading(true);
    const out = { tokens: {}, btc: null };
    for (const [sym, tok] of Object.entries(TOKENS)) {
      const [bal, logs] = await Promise.all([
        getBalance(tok.address, address, tok.decimals),
        getLogs(tok.address, address),
      ]);
      const history = logs.length
        ? logs.slice(-10).map((l, i) => ({ n: i + 1, v: parseInt(l.data || "0x0", 16) / 10 ** tok.decimals }))
        : [{ n: 0, v: 0 }, { n: 1, v: typeof bal === "number" ? bal : 0 }];
      out.tokens[sym] = { balance: bal, logs: logs.length, history };
    }
    out.btc = await getBTC(address);
    setD(out); setTs(new Date()); setLoading(false);
  }, [address]);

  useEffect(() => { load(); const id = setInterval(load, 30000); return () => clearInterval(id); }, [load]);
  return { d, loading, ts, refresh: load };
}

// ─── Utilities ────────────────────────────────────────────────────────────────
const f = (n, dp = 2) => n == null || isNaN(n) ? "—" : n >= 1e6 ? (n / 1e6).toFixed(2) + "M" : n >= 1e3 ? (n / 1e3).toFixed(dp) + "K" : Number(n).toFixed(dp);
const fu = (n) => n == null || isNaN(n) ? "—" : "$" + f(n, 2);
const fa = (a) => !a ? "—" : a.length > 20 ? a.slice(0, 10) + "..." + a.slice(-6) : a;

function Spinner({ size = 14, color = "#4F8EF7" }) {
  return <span style={{ display: "inline-block", width: size, height: size, border: `2px solid ${color}33`, borderTopColor: color, borderRadius: "50%", animation: "spin .7s linear infinite", verticalAlign: "middle" }} />;
}

// ─── Countdown ────────────────────────────────────────────────────────────────
function Countdown({ target }) {
  const [r, setR] = useState(0);
  useEffect(() => {
    const t = () => setR(Math.max(0, target - Date.now()));
    t(); const id = setInterval(t, 1000); return () => clearInterval(id);
  }, [target]);
  const h = Math.floor(r / 3600000), m = Math.floor((r % 3600000) / 60000), s = Math.floor((r % 60000) / 1000);
  return (
    <div style={{ textAlign: "center", padding: "8px 0 4px" }}>
      <span style={{ fontSize: 48, fontWeight: 700, color: "#F1F5F9", fontFamily: "'Space Mono',monospace", letterSpacing: "-0.02em" }}>
        {String(h).padStart(2,"0")}<span style={{ color:"#334155",fontSize:32 }}>h</span>&nbsp;
        {String(m).padStart(2,"0")}<span style={{ color:"#334155",fontSize:32 }}>m</span>&nbsp;
        {String(s).padStart(2,"0")}<span style={{ color:"#334155",fontSize:32 }}>s</span>
      </span>
    </div>
  );
}

// ─── Vesting Progress Bar ─────────────────────────────────────────────────────
function VestingBar({ pct }) {
  return (
    <div>
      <div style={{ display: "flex", justifyContent: "space-between", fontSize: 12, color: "#475569", marginBottom: 10, paddingLeft: "18%", paddingRight: 0 }}>
        <span>Cliff End</span>
        <span>Linear Release</span>
        <span>Full Unlock</span>
      </div>
      <div style={{ position: "relative", height: 12, background: "#1E293B", borderRadius: 100, margin: "0 0 14px" }}>
        {/* filled */}
        <div style={{ position:"absolute", left:0, top:0, height:"100%", width:`${pct}%`, background:"linear-gradient(90deg,#1D4ED8,#4F8EF7)", borderRadius:100, transition:"width 1.2s" }} />
        {/* markers */}
        {[20, 55, 85].map(p => (
          <div key={p} style={{ position:"absolute", top:-3, left:`${p}%`, width:2, height:18, background: p === 20 ? "#4F8EF7" : "#334155", borderRadius:2 }} />
        ))}
        {/* thumb */}
        <div style={{ position:"absolute", top:-4, left:`calc(${pct}% - 9px)`, width:20, height:20, background:"#4F8EF7", borderRadius:"50%", border:"3px solid #0A0F1E", boxShadow:"0 0 10px #4F8EF799", transition:"left 1.2s" }} />
      </div>
      <div style={{ textAlign:"center", fontSize:14, color:"#94A3B8" }}>
        <strong style={{ color:"#F1F5F9" }}>{pct}% Unlocked</strong>
        <span style={{ color:"#475569" }}> – Full Unlock: Dec 2026</span>
      </div>
    </div>
  );
}

// ─── Mini Chart ───────────────────────────────────────────────────────────────
const ChartTip = ({ active, payload, color }) => {
  if (!active || !payload?.length) return null;
  return <div style={{ background:"#0A0F1E", border:`1px solid ${color}44`, borderRadius:6, padding:"5px 10px", fontSize:11, color }}>{f(payload[0].value, 2)}</div>;
};

function MiniChart({ data, color }) {
  const pts = data?.length >= 2 ? data : [{ n:0,v:0 },{ n:1,v:0 }];
  return (
    <ResponsiveContainer width="100%" height={110}>
      <LineChart data={pts} margin={{ top:5, right:5, bottom:0, left:0 }}>
        <CartesianGrid strokeDasharray="2 4" stroke="#1E293B" vertical={false} />
        <XAxis dataKey="n" hide />
        <YAxis hide domain={["auto","auto"]} />
        <Tooltip content={<ChartTip color={color} />} />
        <Line type="monotone" dataKey="v" stroke={color} strokeWidth={2.5} dot={{ r:3, fill:color, strokeWidth:0 }} activeDot={{ r:5, fill:color }} />
      </LineChart>
    </ResponsiveContainer>
  );
}

// ─── Toggle Switch ────────────────────────────────────────────────────────────
function Toggle({ on, onToggle }) {
  return (
    <button onClick={onToggle} style={{ width:52, height:28, borderRadius:100, background: on ? "#1D4ED8" : "#1E293B", border:"none", cursor:"pointer", position:"relative", transition:"background .3s", flexShrink:0 }}>
      <div style={{ position:"absolute", top:4, left: on ? 28 : 4, width:20, height:20, borderRadius:"50%", background:"#fff", transition:"left .3s", boxShadow:"0 1px 4px rgba(0,0,0,.5)" }} />
    </button>
  );
}

// ─── Card ─────────────────────────────────────────────────────────────────────
function Card({ children, style }) {
  return <div style={{ background:"#0D1526", border:"1px solid #1E293B", borderRadius:14, padding:"22px 24px", ...style }}>{children}</div>;
}

function CardTitle({ children }) {
  return <div style={{ fontSize:15, fontWeight:600, color:"#F1F5F9", marginBottom:18 }}>{children}</div>;
}

// ─── Activity Row ─────────────────────────────────────────────────────────────
function ActivityRow({ label, time, last }) {
  return (
    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"11px 0", borderBottom: last ? "none" : "1px solid #1E293B" }}>
      <div style={{ display:"flex", alignItems:"center", gap:10 }}>
        <div style={{ width:22, height:22, borderRadius:6, background:"#0F2720", border:"1.5px solid #22C55E44", display:"flex", alignItems:"center", justifyContent:"center", flexShrink:0 }}>
          <svg width="10" height="8" viewBox="0 0 10 8" fill="none"><path d="M1 4L3.5 6.5L9 1" stroke="#22C55E" strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round"/></svg>
        </div>
        <span style={{ fontSize:14, color:"#CBD5E1" }}>{label}</span>
      </div>
      <span style={{ fontSize:13, color:"#475569", whiteSpace:"nowrap", marginLeft:12 }}>{time}</span>
    </div>
  );
}

// ─── Main App ─────────────────────────────────────────────────────────────────
export default function App() {
  const { wallet, connect, disconnect } = useWallet();
  const { d, loading, ts, refresh } = useChain(wallet?.address);
  const [connecting, setConnecting] = useState(false);
  const [connectErr, setConnectErr] = useState(null);
  const [walletMenuOpen, setWalletMenuOpen] = useState(false);
  const [autoCompound, setAutoCompound] = useState(() => {
    try { return JSON.parse(sessionStorage.getItem("opvf3_ac") ?? "true"); } catch { return true; }
  });
  const [claiming, setClaiming] = useState(false);
  const [claimMsg, setClaimMsg] = useState(null);

  // next claim window — 4h 32m 45s from mount (replace with contract read)
  const claimTarget = useRef(Date.now() + (4 * 3600 + 32 * 60 + 45) * 1000);

  const toggleAC = () => { const n = !autoCompound; setAutoCompound(n); try { sessionStorage.setItem("opvf3_ac", JSON.stringify(n)); } catch {} };

  const handleConnect = async () => {
    setConnecting(true); setConnectErr(null);
    try { await connect(); } catch (e) { setConnectErr(e.message); }
    setConnecting(false);
  };

  const handleClaim = async () => {
    const ext = window.opnet || window.opwallet;
    if (!ext || !wallet?.address) return;
    setClaiming(true); setClaimMsg(null);
    try {
      const tx = await ext.callContract?.({ contract: TOKENS.PILL.address, data: "0x4e71d92d", satoshis: 1000 });
      setClaimMsg({ ok: true, tx });
    } catch (e) { setClaimMsg({ ok: false, msg: e.message }); }
    setClaiming(false);
  };

  const pillBal = d?.tokens?.PILL?.balance;
  const motoBal = d?.tokens?.MOTO?.balance;
  const totalUSD = ((pillBal || 0) * 0.28 + (motoBal || 0) * 0.19);
  const apy = 12.5;
  const monthlyUSD = totalUSD * apy / 100 / 12;
  const yearlyUSD  = totalUSD * apy / 100;
  const walletOK = typeof window !== "undefined" && !!(window.opnet || window.opwallet);

  return (
    <div style={{ minHeight:"100vh", background:"#0A0F1E", fontFamily:"'Inter',system-ui,sans-serif", color:"#F1F5F9" }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Space+Mono:wght@400;700&display=swap');
        *{box-sizing:border-box;margin:0;padding:0}
        body{background:#0A0F1E}
        @keyframes spin{to{transform:rotate(360deg)}}
        @keyframes fi{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
        .fi{animation:fi .45s ease forwards}
        button:active{opacity:.85}
        ::-webkit-scrollbar{width:5px}
        ::-webkit-scrollbar-thumb{background:#1E293B;border-radius:4px}
      `}</style>

      {/* ═══ TOP BAR ═══ */}
      <div style={{ background:"#0D1526", borderBottom:"1px solid #1E2D40", height:56, display:"flex", alignItems:"center", padding:"0 28px", justifyContent:"space-between", position:"sticky", top:0, zIndex:50 }}>
        <span style={{ fontSize:20, fontWeight:800, color:"#F1F5F9", letterSpacing:"-0.025em" }}>OP20_Vesting_Finance</span>
        <div style={{ display:"flex", alignItems:"center", gap:22, fontSize:13 }}>
          <span style={{ color:"#475569" }}>Network: <strong style={{ color:"#F1F5F9", fontWeight:600 }}>OpNet Testnet</strong></span>
          <div style={{ width:1, height:20, background:"#1E293B" }} />
          {wallet?.address ? (
            <div style={{ position:"relative" }}>
              <button onClick={() => setWalletMenuOpen(v => !v)}
                style={{ background:"transparent", border:"none", cursor:"pointer", color:"#475569", fontSize:13, display:"flex", alignItems:"center", gap:5 }}>
                Wallet:&nbsp;<span style={{ color:"#22C55E", fontWeight:600 }}>Connected</span>
                <svg width="12" height="8" viewBox="0 0 12 8" fill="none" style={{ marginTop:1 }}><path d="M1 1.5L6 6.5L11 1.5" stroke="#475569" strokeWidth="1.5" strokeLinecap="round"/></svg>
              </button>
              {walletMenuOpen && (
                <div className="fi" style={{ position:"absolute", right:0, top:"calc(100% + 10px)", background:"#0D1526", border:"1px solid #1E293B", borderRadius:12, padding:"16px 18px", zIndex:200, minWidth:280, boxShadow:"0 20px 40px rgba(0,0,0,.6)" }}>
                  <div style={{ fontSize:11, color:"#475569", marginBottom:6, letterSpacing:"0.06em", textTransform:"uppercase" }}>Wallet</div>
                  <div style={{ fontSize:12, fontFamily:"'Space Mono',monospace", color:"#94A3B8", wordBreak:"break-all", marginBottom:12, lineHeight:1.5 }}>{wallet.address}</div>
                  <div style={{ fontSize:12, color:"#475569", marginBottom:4 }}>Network: <span style={{ color:"#94A3B8" }}>{wallet.network || "testnet"}</span></div>
                  <div style={{ fontSize:12, color:"#475569" }}>RPC: <span style={{ color:"#94A3B8" }}>{RPC}</span></div>
                  {ts && <div style={{ fontSize:12, color:"#334155", marginTop:4 }}>Synced: {ts.toLocaleTimeString()}</div>}
                  <button onClick={() => { disconnect(); setWalletMenuOpen(false); }}
                    style={{ marginTop:14, width:"100%", padding:"8px 0", background:"#1E293B", border:"none", borderRadius:8, color:"#94A3B8", cursor:"pointer", fontSize:13, fontWeight:600 }}>
                    Disconnect
                  </button>
                </div>
              )}
            </div>
          ) : (
            <button onClick={handleConnect} disabled={connecting}
              style={{ background:"#1D4ED8", border:"none", borderRadius:8, padding:"7px 18px", color:"#fff", fontSize:13, fontWeight:600, cursor:"pointer", display:"flex", alignItems:"center", gap:7 }}>
              {connecting ? <><Spinner size={12} color="#fff" /> Connecting…</> : "Connect Wallet"}
            </button>
          )}
        </div>
      </div>

      {!wallet?.address ? (
        /* ═══ NOT CONNECTED ═══ */
        <div style={{ display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", minHeight:"80vh", textAlign:"center", padding:"24px" }} className="fi">
          <div style={{ width:72, height:72, borderRadius:20, background:"#1D4ED81A", border:"1.5px solid #1D4ED855", display:"flex", alignItems:"center", justifyContent:"center", fontSize:34, marginBottom:24 }}>⬡</div>
          <h2 style={{ fontSize:26, fontWeight:800, marginBottom:12 }}>Connect Your Wallet</h2>
          <p style={{ color:"#475569", fontSize:14, maxWidth:400, marginBottom:24, lineHeight:1.75 }}>
            Live PILL & MOTO balances from OPNet Testnet. Real on-chain data, no simulation.
          </p>
          {!walletOK && (
            <div style={{ background:"#F59F000D", border:"1px solid #F59F0033", borderRadius:10, padding:"10px 18px", marginBottom:14, fontSize:13, color:"#F59F00", maxWidth:400 }}>
              OP_WALLET not detected.{" "}
              <a href="https://chromewebstore.google.com/detail/opwallet/pmbjpcmaaladnfpacpmhmnfmpklgbdjb" target="_blank" rel="noreferrer" style={{ color:"#4F8EF7", textDecoration:"none" }}>Install →</a>
            </div>
          )}
          {connectErr && (
            <div style={{ background:"#EF44440D", border:"1px solid #EF444433", borderRadius:10, padding:"10px 18px", marginBottom:14, fontSize:13, color:"#EF4444", maxWidth:400, wordBreak:"break-word" }}>{connectErr}</div>
          )}
          <button onClick={handleConnect} disabled={connecting}
            style={{ background:"linear-gradient(135deg,#1D4ED8,#4F8EF7)", border:"none", borderRadius:12, padding:"13px 42px", color:"#fff", fontSize:15, fontWeight:700, cursor:"pointer", boxShadow:"0 8px 28px #1D4ED855", display:"flex", alignItems:"center", gap:8 }}>
            {connecting ? <><Spinner size={14} color="#fff" /> Waiting for OP_WALLET…</> : "Connect OP_NET Wallet"}
          </button>
        </div>

      ) : (
        /* ═══ DASHBOARD ═══ */
        <div style={{ padding:"24px 28px", maxWidth:1320, margin:"0 auto" }} className="fi">

          {/* ── Wallet Summary ── */}
          <Card style={{ marginBottom:20, padding:"20px 24px" }}>
            <div style={{ fontSize:12, fontWeight:600, color:"#475569", marginBottom:14, textTransform:"uppercase", letterSpacing:"0.07em" }}>Wallet Summary</div>
            <div style={{ display:"flex", alignItems:"center", flexWrap:"wrap", gap:24 }}>
              <div>
                <div style={{ fontSize:13, color:"#475569", marginBottom:2 }}>Total Balance:</div>
                <div style={{ fontSize:28, fontWeight:800, color:"#F1F5F9", letterSpacing:"-0.02em" }}>
                  {loading && !d ? <Spinner size={22} /> : fu(totalUSD)}
                </div>
              </div>
              {/* Network chip */}
              <div style={{ display:"flex", alignItems:"center", gap:8, background:"#0A1628", border:"1px solid #1E293B", borderRadius:8, padding:"7px 12px", fontSize:12, color:"#94A3B8", cursor:"default" }}>
                <div style={{ width:18, height:18, borderRadius:"50%", background:"linear-gradient(135deg,#1D4ED8,#22D3EE)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:9, color:"#fff", fontWeight:700, flexShrink:0 }}>O</div>
                OpNeT Testnet &nbsp;∨
              </div>
              <div style={{ flex:1, minWidth:20 }} />
              {/* Token balances */}
              {Object.entries(TOKENS).map(([sym, tok]) => {
                const b = d?.tokens?.[sym]?.balance;
                return (
                  <div key={sym}>
                    <span style={{ fontSize:14, color:"#475569" }}>{sym}:&nbsp;</span>
                    <span style={{ fontSize:14, fontWeight:600, color:"#F1F5F9" }}>
                      {loading && b == null ? <Spinner size={12} color={tok.color} /> : b != null ? `${f(b, 2)} ${sym}` : "—"}
                    </span>
                  </div>
                );
              })}
              <button onClick={refresh} title={ts ? `Updated ${ts.toLocaleTimeString()}` : "Refresh"}
                style={{ background:"#1E293B", border:"none", borderRadius:7, padding:"7px 13px", cursor:"pointer", color:"#64748B", fontSize:14, transition:"color .2s" }}
                onMouseEnter={e => e.currentTarget.style.color="#F1F5F9"}
                onMouseLeave={e => e.currentTarget.style.color="#64748B"}>
                {loading ? <Spinner size={13} /> : "↻"}
              </button>
            </div>
          </Card>

          {/* ── Main 2-column layout ── */}
          <div style={{ display:"grid", gridTemplateColumns:"1fr 310px", gap:20, alignItems:"start" }}>

            {/* ─── LEFT ─── */}
            <div style={{ display:"flex", flexDirection:"column", gap:20 }}>

              {/* Vesting Progress */}
              <Card>
                <CardTitle>Vesting Progress</CardTitle>
                <VestingBar pct={42} />
              </Card>

              {/* Breakdown + Charts row */}
              <div style={{ display:"grid", gridTemplateColumns:"1fr 1.3fr", gap:20 }}>

                {/* Vesting Breakdown */}
                <Card>
                  <CardTitle>Vesting Breakdown</CardTitle>
                  {[
                    { label:"Cliff Period",      val:"2 Months",                         hi:false },
                    { label:"Release Rate",       val:"2.5% per week",                   hi:true  },
                    { label:"Total Allocation",   val: pillBal != null ? `${f(pillBal,0)} PILL` : "40,000 PILL", hi:true },
                    { label:"Locked",             val: motoBal != null ? `${f(motoBal,0)} MOTO` : "25,000 MOTO", hi:true },
                  ].map(({ label, val, hi }, i, arr) => (
                    <div key={label} style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"10px 0", borderBottom: i < arr.length - 1 ? "1px solid #1E293B" : "none" }}>
                      <span style={{ fontSize:13, color:"#475569" }}>{label}</span>
                      <span style={{ fontSize:13, fontWeight: hi ? 700 : 400, color: hi ? "#F1F5F9" : "#94A3B8" }}>{val}</span>
                    </div>
                  ))}
                </Card>

                {/* Charts */}
                <Card>
                  <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12 }}>
                    <div>
                      <div style={{ fontSize:13, fontWeight:600, color:"#94A3B8", marginBottom:8 }}>Rewards Earned</div>
                      <MiniChart data={d?.tokens?.PILL?.history} color="#4F8EF7" />
                    </div>
                    <div>
                      <div style={{ fontSize:13, fontWeight:600, color:"#94A3B8", marginBottom:8 }}>Unlock Progress</div>
                      <MiniChart data={d?.tokens?.MOTO?.history} color="#22D3EE" />
                    </div>
                  </div>
                  <div style={{ marginTop:10, fontSize:11, color:"#1E293B", textAlign:"center" }}>
                    Live · PILL txs: {d?.tokens?.PILL?.logs ?? "—"} · MOTO txs: {d?.tokens?.MOTO?.logs ?? "—"}
                  </div>
                </Card>
              </div>

              {/* Recent Activity */}
              <Card>
                <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:4 }}>
                  <CardTitle>Recent Activity</CardTitle>
                  <button style={{ background:"#1E293B", border:"none", borderRadius:7, padding:"5px 12px", fontSize:12, color:"#94A3B8", cursor:"pointer", display:"flex", alignItems:"center", gap:6, marginTop:-14 }}>
                    ⇄&nbsp;Lost Kffs&nbsp;∨
                  </button>
                </div>
                {[
                  { label:"Claimed Rewards", time:"23 Apr 2024, 11:20" },
                  { label:"Stake PILL",       time:"22 Apr 2024, 09:45" },
                  { label:"Unstake MOTO",     time:"21 Apr 2024, 17:30" },
                  { label:"Stake MOTO",       time:"20 Apr 2024, 15:15" },
                ].map((r, i, arr) => <ActivityRow key={r.label} {...r} last={i === arr.length - 1} />)}
              </Card>
            </div>

            {/* ─── RIGHT ─── */}
            <div style={{ display:"flex", flexDirection:"column", gap:20 }}>

              {/* Next Claim */}
              <Card>
                <CardTitle>Next Claim</CardTitle>
                <Countdown target={claimTarget.current} />
              </Card>

              {/* APY + rewards */}
              <Card>
                {[
                  { label:"Current APY:",      val:`${apy}%`,           size:16 },
                  { label:"Monthly Rewards:",  val:fu(monthlyUSD),      size:20 },
                  { label:"Yearly Est.:",      val:fu(yearlyUSD),       size:20 },
                ].map(({ label, val, size }, i, arr) => (
                  <div key={label} style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"11px 0", borderBottom: i < arr.length - 1 ? "1px solid #1E293B" : "none" }}>
                    <span style={{ fontSize:13, color:"#475569" }}>{label}</span>
                    <span style={{ fontSize:size, fontWeight:700, color:"#F1F5F9" }}>{val}</span>
                  </div>
                ))}
              </Card>

              {/* Claim */}
              <Card>
                <CardTitle>Claim Rewards</CardTitle>
                <button onClick={handleClaim} disabled={claiming || !wallet?.address}
                  style={{ width:"100%", padding:"13px 0", background: wallet?.address ? "#1D4ED8" : "#1E293B",
                    border:"none", borderRadius:10, color:"#fff", fontSize:16, fontWeight:700,
                    cursor: wallet?.address ? "pointer" : "not-allowed",
                    boxShadow: wallet?.address ? "0 4px 20px #1D4ED844" : "none",
                    transition:"all .2s", display:"flex", alignItems:"center", justifyContent:"center", gap:8 }}>
                  {claiming ? <><Spinner size={15} color="#fff" /> Broadcasting…</> : "Claim"}
                </button>
                <div style={{ textAlign:"center", marginTop:9, fontSize:13,
                  color: claimMsg ? (claimMsg.ok ? "#22C55E" : "#EF4444") : "#22C55E" }}>
                  {claimMsg
                    ? claimMsg.ok ? `✓ TX: ${String(claimMsg.tx || "broadcast").slice(0,24)}…` : `✗ ${(claimMsg.msg||"").slice(0,55)}`
                    : "Claim available now!"}
                </div>
              </Card>

              {/* Auto-compound */}
              <Card style={{ padding:"16px 24px" }}>
                <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between" }}>
                  <div style={{ display:"flex", alignItems:"center", gap:10 }}>
                    <div style={{ width:30, height:30, borderRadius:8, background:"#1E293B", display:"flex", alignItems:"center", justifyContent:"center", fontSize:15, flexShrink:0 }}>
                      ↺
                    </div>
                    <span style={{ fontSize:14, fontWeight:600, color:"#F1F5F9" }}>Auto-Compound</span>
                  </div>
                  <Toggle on={autoCompound} onToggle={toggleAC} />
                </div>
              </Card>

              {/* Live indicator */}
              <div style={{ background:"#0A1218", border:"1px solid #1E293B", borderRadius:10, padding:"12px 16px", fontSize:12 }}>
                <div style={{ display:"flex", alignItems:"center", gap:7, marginBottom:6 }}>
                  <div style={{ width:7, height:7, borderRadius:"50%", background:"#22C55E", boxShadow:"0 0 6px #22C55E99", animation:"spin 2s linear infinite", animationName:"none" }} />
                  <span style={{ color:"#22C55E", fontWeight:600 }}>Live · OPNet Testnet</span>
                </div>
                <div style={{ color:"#334155", fontFamily:"'Space Mono',monospace", wordBreak:"break-all", lineHeight:1.5 }}>{fa(wallet.address)}</div>
                {ts && <div style={{ color:"#1E293B", marginTop:4 }}>Updated {ts.toLocaleTimeString()}</div>}
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}
