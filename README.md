# Voibandiera.github.io
import React, { useMemo, useState } from "react";
const CONFIG = {
  siteName: "Voibandiera",
  currency: "₺",
  bankTransfer: {
    iban: "TR370006701000000024043113",
    accountName: "Toprak Bayraktar",
    note: "Sipariş No: {ORDER_ID}"
  },
  paymentLinks: {
    "dropshipping-baslangic-egitimi": "https://example.com/odeme/dropshipping-baslangic-egitimi"
  },
  products: [
    {
      slug: "dropshipping-baslangic-egitimi",
      title: "Dropshipping Başlangıç Eğitimi",
      price: 99.90,
      image: "https://lh3.googleusercontent.com/rd-gg/AAHar4ddQMOR94EL0wVGZ_Vps8tK0vh-6UkZ4miJkuBE-kDeu-Kjr_GZrvIXAt25TjWi89LK-0_7g9Bn1QXLkeB6cWzh-xj3dc4DKfSvJttt82YJMEMWcbzt_yVoZTQNYc08AGxz_LIuYzFAeJCSzhpEHqnU6_-OjR7-Pa8q4xibKDmg7N3sz22aT9SEncPDnvn_Eic2HbIlSNIUroKxSY70EvyKXLIe_gASDUSZcqhV67tUx-urTkBcx9oG2GQoc_gnKaqHiaRCAmb7ANKV9Tn16N8cvuRiQXzdJ5IkTacJ50yOkzTmkH8a6tDi5Koj6o--d3UamjTtIPXcsBKhNp70kUrapo1k7D9POxOnaDMTvZ4qcM2k8QS3Bn3-_oK_bAVrSFNYbgiMUPZuA0vF_GQ_Z2mnTw4QM_6blkpDZUt2LUHLrrOgL1vYdojryFUJuJcS-raH_leyOEn2lRjwtUOa-s_3uxLmgKn8rcpddRngXkGENluG1Y2EJYkrqmOIxt90Kz-28Yni6JH2uhdZPNseV0JCJzaqePY6nM-bWiVHXB1-LrIlpPE3KtT8weqUWAEJrX8AmViJpodUspSOhnenhOQ67cqM7puC93nfzvK-bCIF6a_M8U2-j8dGiMDIxZxhCCx687MpghuU-k3t0aAAkJTl6WOC8cyC33GDtpmwLCbe0yp6_IS_1TS2zVvI681noDOaEaaeNTmUjcS7UoTgREbkmuWzMVip0t0O30GS-79W8oYa9bBfdSVBEI2-I_FwnItJovRvCvqvQS3RjDxqbfAcSEKY81Cfs1RKbaFlSccWL2ofpNSJDSOLkFyovPjjdmNL55I1oiZBclq594ejwnlzs3LpqF7b0B1bU7_PzHdfq2lGv_rHD5gZesYYPDiCOOIK8RDHSS60f7zK0cpihJQZudYhxgFkwCyWCwPS2YsW3C5X4bcfRoGA0V2N2g2hkKywFrXgFxK3NCm8DEIzGGlFQdeoOtVqZQSGCaZ5jumfXru0oCatVcilEKe-P_nHOuGoqVxmxo2xMR1goKdziROcvFcshiYSYbkdRZlS_46njHHYLf9PTZF2crq5wUxtqW5DO88rjLUQmLr9drAxf7jGV4ZvJSv2KyHMotyZlmcqcsHc7dYTqwn-VoZv90uIYUoWT6eBnVbUi2bsme9HMhEHsVjHBzjvk_Rh0P75x8u333sDk04fNs4F0Ab68eMvvt7_m3f0YG-KtBX_vpJic52KDfJA1lC9FFyLsYTu_6YEi6_3cWQC7YQsWXiZMUkcQo5_A11WdlhAgHeGyfz4jH7FZswsAuLSRDjz9Qk0Tfg3mrg60PCEIlwdi80Tf86OxGjtV8egNeUFwnSNzCbXjjGeecUHhuPWQ_G9R-vkAopzEY2KJFe8jA9ZiSSONXJyWF5UdMNiCzAYBMEAorp-q4QPpa8__wsi-yLkwloC4fJbmnfdDuXeVPN18NOBlEVXgGlgfghaBUxQd2nHQr44Lkqaz2-n-rSLkRsptW0TM0t1CP5M55pFtMhkyzfA93gDkeg1lCGwh9SC8CIH9QVObR1H5lv8kpw3XGFzfZRYFyz5UCAAYO51Gw=s512",
      description: "Yeni başlayanlar için kapsamlı dropshipping eğitimi."
    }
  ]
};

const uid = () => Math.random().toString(36).slice(2, 10);

function useCart() {
  const [items, setItems] = useState([]);
  const add = (slug) => {
    setItems(prev => {
      const i = prev.find(x => x.slug === slug);
      if(i) return prev.map(x => x.slug === slug ? {...x, qty: x.qty+1} : x);
      return [...prev, {slug, qty: 1}];
    });
  };
  const remove = (slug) => setItems(prev => prev.filter(x => x.slug !== slug));
  const inc = (slug) => setItems(prev => prev.map(x => x.slug===slug ? {...x, qty:x.qty+1} : x));
  const dec = (slug) => setItems(prev => prev.map(x => x.slug===slug ? {...x, qty:x.qty-1} : x).filter(x => x.qty>0));

  const detailed = useMemo(() => {
    const bySlug = Object.fromEntries(CONFIG.products.map(p => [p.slug, p]));
    const rows = items.map(x => ({...bySlug[x.slug], qty:x.qty}));
    const total = rows.reduce((s,r) => s + r.price*r.qty, 0);
    return {rows, total};
  }, [items]);

  const clear = () => setItems([]);
  return {items, add, remove, inc, dec, detailed, clear};
}

function Badge({children}) {
  return <span className="inline-flex items-center rounded-full border px-2 py-0.5 text-xs text-gray-600">{children}</span>;
}

function Button({children, className="", ...props}) {
  return <button className={"rounded-2xl px-4 py-2 shadow-sm transition active:scale-[0.99] bg-black text-white hover:opacity-90 "+className} {...props}>{children}</button>;
}

function OutlineButton({children, className="", ...props}) {
  return <button className={"rounded-2xl px-4 py-2 border shadow-sm transition active:scale-[0.99] bg-white text-gray-900 hover:bg-gray-50 "+className} {...props}>{children}</button>;
}

function CopyField({label,value}) {
  const [copied, setCopied] = useState(false);
  return (
    <div className="space-y-1">
      <div className="text-xs text-gray-500">{label}</div>
      <div className="flex items-center gap-2">
        <input className="w-full rounded-xl border px-3 py-2 bg-gray-50" readOnly value={value} onFocus={e=>e.target.select()} />
        <OutlineButton onClick={async()=>{await navigator.clipboard.writeText(value); setCopied(true); setTimeout(()=>setCopied(false),1200)}}>{copied ? "Kopyalandı" : "Kopyala"}</OutlineButton>
      </div>
    </div>
  );
}

function PaymentModal({open,onClose,amount,orderId}) {
  if(!open) return null;
  const {iban, accountName, note} = CONFIG.bankTransfer;
  const noteText = note?.replace("{ORDER_ID}", orderId) ?? `Sipariş No: ${orderId}`;
  return (
    <div className="fixed inset-0 z-50 grid place-items-center bg-black/40 p-4" onClick={onClose}>
      <div className="w-full max-w-lg rounded-3xl bg-white p-6 shadow-xl" onClick={e=>e.stopPropagation()}>
        <div className="flex items-start justify-between">
          <h3 className="text-lg font-semibold">Havale/EFT ile Öde</h3>
          <button className="text-gray-400" onClick={onClose} aria-label="kapat">✕</button>
        </div>
        <div className="mt-4 space-y-4">
          <Badge>ÜCRETSİZ altyapı • Banka transferi</Badge>
          <div className="text-2xl font-bold">Toplam: {CONFIG.currency}{amount.toLocaleString("tr-TR")}</div>
          <CopyField label="IBAN" value={iban} />
          <CopyField label="Hesap Adı" value={accountName} />
          <CopyField label="Açıklama" value={noteText} />
          <div className="text-xs text-gray-500">Not: Bilgileri kopyalayarak yapıştırın.</div>
        </div>
      </div>
    </div>
  );
}

function App() {
  const cart = useCart();
  const [payOpen,setPayOpen] = useState(false);
  const [orderId] = useState(()=>uid());

  const handlePay = providerLink=>{if(providerLink){window.open(providerLink,"_blank","noopener,noreferrer");}else{alert("Bu ürün için ödeme linki eklenmemiş.");}};
  const handleBankTransfer = ()=>setPayOpen(true);

  return (
    <div className="min-h-screen bg-gradient-to-b from-gray-50 to-white text-gray-900">
      <header className="sticky top-0 z-40 backdrop-blur bg-white/60 border-b">
        <div className="mx-auto max-w-6xl px-4 py-4 flex items-center justify-between">
          <div className="flex items-center gap-2">
            <div className="h-8 w-8 rounded-2xl bg-black text-white grid place-items-center font-bold">T</div>
            <div className="font-semibold">{CONFIG.siteName}</div>
          </div>
          <div className="flex items-center gap-2">
            <Badge>Sipariş No: {orderId.toUpperCase()}</Badge>
          </div>
        </div>
      </header>

      <section className="mx-auto max-w-6xl px-4 py-10 md:py-14">
        <div className="grid gap-8 md:grid-cols-2 items-center">
          <div className="space-y-4">
            <h1 className="text-3xl md:text-5xl font-extrabold leading-tight">Sıfır Backend, Ücretsiz Ödeme Yönlendirme</h1>
            <p className="text-gray-600 text-lg">Kart verisini biz toplamıyoruz. Güvenli sağlayıcı sayfasına yönlendiriyoruz. İsterseniz tamamen ücretsiz Havale/EFT seçeneği de var.</p>
            <div className="flex gap-3">
              <Button onClick={()=>document.getElementById("products")?.scrollIntoView({behavior:"smooth"})}>Ürünlere Göz At</Button>
              <OutlineButton onClick={handleBankTransfer}>IBAN ile Öde</OutlineButton>
            </div>
          </div>
          <div className="rounded-3xl overflow-hidden shadow-2xl border">
            <img src="https://images.unsplash.com/photo-1517245386807-bb43f82c33c4?q=80&w=1600&auto=format&fit=crop" alt="Hero görsel" className="w-full h-full object-cover" />
          </div>
        </div>
      </section>

      <section id="products" className="mx-auto max-w-6xl px-4 pb-20">
        <div className="mb-6 flex items-end justify-between">
          <h2 className="text-2xl md:text-3xl font-bold">Ürünler</h2>
          <Badge>Sepet: {CONFIG.currency}{cart.detailed.total.toLocaleString("tr-TR")}</Badge>
        </div>
        <div className="grid gap-6 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3">
          {CONFIG.products.map(p=>(
            <div key={p.slug} className="rounded-3xl border shadow-sm overflow-hidden bg-white flex flex-col">
              <img src={p.image} alt={p.title} className="h-56 w-full object-cover" />
              <div className="p-4 flex-1 flex flex-col">
                <div className="flex items-start justify-between gap-3">
                  <div>
                    <div className="text-lg font-semibold">{p.title}</div>
                    <div className="text-sm text-gray-500">{p.description}</div>
                  </div>
                  <div className="text-right font-bold">{CONFIG.currency}{p.price.toLocaleString("tr-TR")}</div>
                </div>
                <div className="mt-4 flex gap-2">
                  <Button className="flex-1" onClick={()=>cart.add(p.slug)}>Sepete Ekle</Button>
                  <OutlineButton className="flex-1" onClick={()=>handlePay(CONFIG.paymentLinks[p.slug])}>Satın Al</OutlineButton>
                </div>
              </div>
            </div>
          ))}
        </div>
      </section>

      <div className="sticky bottom-3 z-40">
        <div className="mx-auto max-w-3xl rounded-3xl bg-white border shadow-xl p-3 flex items-center gap-3">
          <div className="flex-1 truncate">
            {cart.detailed.rows.length===0 ? <span className="text-gray-500">Sepetiniz boş</span> : <span>{cart.detailed.rows.map(r=>`${r.title} × ${r.qty}`).join(" • ")}</span>}
          </div>
          <div className="font-bold">{CONFIG.currency}{cart.detailed.total.toLocaleString("tr-TR")}</div>
          <OutlineButton onClick={()=>cart.clear()}>Temizle</OutlineButton>
          <Button onClick={handleBankTransfer}>IBAN ile Öde</Button>
        </div>
      </div>

      <footer className="mt-16 border-t">
        <div className="mx-auto max-w-6xl px-4 py-8 text-sm text-gray-500 flex flex-wrap items-center justify-between gap-3">
          <div>© {new Date().getFullYear()} {CONFIG.siteName}. Tüm hakları saklıdır.</div>
          <div className="flex items-center gap-2">
            <Badge>PCI dışı</Badge>
            <Badge>SSL önerilir</Badge>
            <Badge>Statik hosting</Badge>
          </div>
        </div>
      </footer>

      <PaymentModal open={payOpen} onClose={()=>setPayOpen(false)} amount={cart.detailed.total} orderId={orderId} />
    </div>
  );
}

export default App;
