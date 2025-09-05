# SmartBMI
SmartBMI adalah teman sehatmu — hitung BMI dengan mudah, temukan motivasi, dan mulai langkah kecil menuju perubahan besar.
import React, { useEffect, useMemo, useState } from "react";

// SmartBMI Web App (React + Tailwind)
// — Bahasa Indonesia —
// Tema biru, inspiratif, kalkulator BMI

export default function App() {
  const [heightCm, setHeightCm] = useState(() => {
    const v = localStorage.getItem("bmi_height_cm");
    return v ? Number(v) : "";
  });
  const [weightKg, setWeightKg] = useState(() => {
    const v = localStorage.getItem("bmi_weight_kg");
    return v ? Number(v) : "";
  });
  const [touched, setTouched] = useState(false);

  useEffect(() => {
    if (heightCm) localStorage.setItem("bmi_height_cm", String(heightCm));
    if (weightKg) localStorage.setItem("bmi_weight_kg", String(weightKg));
  }, [heightCm, weightKg]);

  const result = useMemo(() => {
    const h = Number(heightCm);
    const w = Number(weightKg);

    if (!h || !w || h <= 0 || w <= 0) return null;
    const m = h / 100;
    const bmi = w / (m * m);

    let category = "";
    let color = "";
    let advice = "";

    if (bmi < 18.5) {
      category = "Kurus";
      color = "text-sky-600";
      advice =
        "Coba tingkatkan asupan kalori bernutrisi (protein, karbo kompleks, lemak sehat) dan latihan kekuatan. Konsultasi ke ahli gizi bila perlu.";
    } else if (bmi < 25) {
      category = "Normal";
      color = "text-blue-600";
      advice =
        "Pertahankan pola makan seimbang, aktif bergerak 150–300 menit/minggu, dan tidur cukup.";
    } else if (bmi < 30) {
      category = "Berat badan berlebih";
      color = "text-indigo-600";
      advice =
        "Mulai defisit kalori ringan, perbanyak sayur-buah, kurangi minuman manis, tambah jalan/olahraga.";
    } else {
      category = "Obesitas";
      color = "text-rose-600";
      advice =
        "Fokus pada perubahan bertahap: porsi terukur, makanan utuh, aktivitas rutin. Pertimbangkan konsultasi medis.";
    }

    return { bmi: Number(bmi.toFixed(1)), category, color, advice };
  }, [heightCm, weightKg]);

  const errorMsg = useMemo(() => {
    if (!touched) return "";
    const h = Number(heightCm);
    const w = Number(weightKg);
    if (!h || !w) return "Isi tinggi & berat dulu ya.";
    if (h < 80 || h > 250) return "Tinggi tampaknya tidak realistis (80–250 cm).";
    if (w < 20 || w > 300) return "Berat tampaknya tidak realistis (20–300 kg).";
    return "";
  }, [touched, heightCm, weightKg]);

  const resetForm = () => {
    setHeightCm("");
    setWeightKg("");
    setTouched(false);
  };

  return (
    <div className="min-h-screen bg-gradient-to-b from-blue-50 to-white text-slate-800">
      {/* NAV */}
      <header className="sticky top-0 z-10 backdrop-blur bg-white/70 border-b border-blue-200">
        <div className="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
          <h1 className="text-xl md:text-2xl font-bold tracking-tight text-blue-700">SmartBMI</h1>
          <a
            href="#inspirasi"
            className="text-sm md:text-base text-blue-600 hover:underline underline-offset-4"
          >
            Inspirasi
          </a>
        </div>
      </header>

      {/* HERO */}
      <section className="max-w-6xl mx-auto px-4 py-8 md:py-12 grid md:grid-cols-2 gap-8 items-center">
        <div>
          <h2 className="text-2xl md:text-4xl font-extrabold leading-tight">
            Cek BMI-mu di <span className="bg-clip-text text-transparent bg-gradient-to-r from-sky-600 to-blue-700">SmartBMI</span>, mulai kebiasaan sehat hari ini.
          </h2>
          <p className="mt-3 text-slate-600">
            Body Mass Index (BMI) adalah estimasi komposisi tubuh berdasarkan tinggi dan berat. Hasilnya bukan diagnosis, tapi bisa jadi titik awal untuk perubahan gaya hidup.
          </p>
        </div>
        <div className="relative rounded-3xl overflow-hidden shadow-lg">
          {/* Gambar inspiratif (Unsplash) */}
          <img
            src="https://images.unsplash.com/photo-1554284126-aa88f22d8b74?q=80&w=1600&auto=format&fit=crop"
            alt="Orang berlari di pagi hari—semangat hidup sehat"
            className="w-full h-64 md:h-80 object-cover"
            loading="lazy"
          />
          <div className="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent" />
          <div className="absolute bottom-3 left-4 right-4 text-white">
            <p className="text-sm uppercase tracking-widest/loose opacity-90">#StayActive</p>
            <p className="text-lg md:text-xl font-semibold">
              "Langkah kecil hari ini, perubahan besar esok hari."
            </p>
          </div>
        </div>
      </section>

      {/* MAIN CONTENT */}
      <main className="max-w-6xl mx-auto px-4 pb-16 grid md:grid-cols-2 gap-8">
        {/* Kalkulator */}
        <section className="bg-white rounded-3xl shadow-lg p-6 md:p-8 border border-blue-200">
          <h3 className="text-xl md:text-2xl font-bold text-blue-700">Hitung BMI</h3>
          <p className="text-sm text-slate-600 mt-1">Gunakan satuan metrik (cm & kg).</p>

          <div className="mt-6 space-y-4">
            <div>
              <label htmlFor="height" className="block text-sm font-medium">Tinggi badan (cm)</label>
              <input
                id="height"
                type="number"
                inputMode="decimal"
                min={80}
                max={250}
                placeholder="mis. 165"
                value={heightCm}
                onChange={(e) => setHeightCm(e.target.value)}
                onBlur={() => setTouched(true)}
                className="mt-1 w-full rounded-xl border border-blue-300 px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>
            <div>
              <label htmlFor="weight" className="block text-sm font-medium">Berat badan (kg)</label>
              <input
                id="weight"
                type="number"
                inputMode="decimal"
                min={20}
                max={300}
                placeholder="mis. 60"
                value={weightKg}
                onChange={(e) => setWeightKg(e.target.value)}
                onBlur={() => setTouched(true)}
                className="mt-1 w-full rounded-xl border border-blue-300 px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>

            {errorMsg && (
              <div className="text-sm text-rose-600 bg-rose-50 border border-rose-200 rounded-xl p-3">
                {errorMsg}
              </div>
            )}

            <div className="flex gap-3 pt-2">
              <button
                type="button"
                onClick={() => setTouched(true)}
                className="rounded-xl px-4 py-2 bg-blue-600 text-white font-semibold shadow hover:bg-blue-700 active:scale-[0.99]"
                aria-label="Hitung BMI"
              >
                Hitung
              </button>
              <button
                type="button"
                onClick={resetForm}
                className="rounded-xl px-4 py-2 bg-slate-100 text-slate-700 font-semibold border border-blue-200 hover:bg-blue-50"
              >
                Reset
              </button>
            </div>

            {/* Hasil */}
            <div className="mt-4">
              {result ? (
                <div className="space-y-3">
                  <div className="flex items-baseline gap-3">
                    <p className="text-5xl font-extrabold tracking-tight text-blue-700">
                      {result.bmi}
                    </p>
                    <p className={`text-lg font-semibold ${result.color}`}>{result.category}</p>
                  </div>
                  <div className="w-full bg-slate-200 h-3 rounded-full overflow-hidden">
                    {/* Progress bar relatif dari 15–35 untuk visual kasar */}
                    <div
                      className="h-full bg-gradient-to-r from-sky-400 via-blue-500 to-indigo-600"
                      style={{
                        width: `${Math.min(100, Math.max(0, ((result.bmi - 15) / (35 - 15)) * 100))}%`,
                      }}
                    />
                  </div>
                  <p className="text-sm text-slate-600">
                    Rentang sehat (dewasa): <span className="font-medium">18.5–24.9</span>
                  </p>
                  <p className="text-sm text-slate-700">{result.advice}</p>
                </div>
              ) : (
                <p className="text-slate-600">Masukkan tinggi & berat untuk melihat hasil.</p>
              )}
            </div>
          </div>

          <details className="mt-6 group">
            <summary className="cursor-pointer select-none text-sm text-slate-600 hover:text-slate-800">
              Bagaimana cara menghitung BMI?
            </summary>
            <div className="mt-2 text-sm text-slate-700 bg-blue-50 rounded-xl p-4 border border-blue-200">
              BMI = berat (kg) ÷ (tinggi (m))². Contoh: 60 kg dan 1,65 m → 60 ÷ 1,65² = 22,0.
              Ingat, BMI tidak mempertimbangkan massa otot/lemak secara langsung.
            </div>
          </details>
        </section>

        {/* Panel Inspirasi */}
        <section id="inspirasi" className="space-y-4">
          <div className="relative overflow-hidden rounded-3xl border border-blue-200 shadow-lg">
            <img
              src="https://images.unsplash.com/photo-1517836357463-d25dfeac3438?q=80&w=1600&auto=format&fit=crop"
              alt="Latihan kekuatan di gym—menjaga kebugaran"
              className="w-full h-56 md:h-64 object-cover"
              loading="lazy"
            />
            <div className="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent" />
            <div className="absolute bottom-3 left-4 right-4 text-white">
              <p className="text-sm uppercase tracking-widest/loose opacity-90">#ConsistentNotPerfect</p>
              <p className="text-lg md:text-xl font-semibold">
                "Konsisten itu lebih kuat daripada motivasi sesaat."
              </p>
            </div>
          </div>

          <ul className="grid gap-4 md:grid-cols-2">
            {[
              {
                src: "https://images.unsplash.com/photo-1506806732259-39c2d0268443?q=80&w=1200&auto=format&fit=crop",
                caption: "Pilih makanan utuh berwarna-warni."
              },
              {
                src: "https://images.unsplash.com/photo-1549576490-b0b4831ef60a?q=80&w=1200&auto=format&fit=crop",
                caption: "Air putih dulu, kopi/teh kemudian."
              },
              {
                src: "https://images.unsplash.com/photo-1571019614242-c5c5dee9f50b?q=80&w=1200&auto=format&fit=crop",
                caption: "Gerak 7.000–10.000 langkah per hari."
              },
              {
                src: "https://images.unsplash.com/photo-1518310383802-640c2de311b2?q=80&w=1200&auto=format&fit=crop",
                caption: "Tidur cukup memperbaiki hormon lapar."
              },
            ].map((item, idx) => (
              <li key={idx} className="relative overflow-hidden rounded-2xl border border-blue-200">
                <img src={item.src} alt={item.caption} className="w-full h-40 object-cover" loading="lazy" />
                <div className="absolute inset-0 bg-black/20" />
                <p className="absolute bottom-2 left-3 right-3 text-white text-sm font-medium drop-shadow">
                  {item.caption}
                </p>
              </li>
            ))}
          </ul>

          <div className="text-xs text-slate-500">
            Foto dari Unsplash. Informasi BMI bersifat umum dan tidak menggantikan saran tenaga kesehatan.
          </div>
        </section>
      </main>

      {/* FOOTER */}
      <footer className="border-t border-blue-200 py-6 text-center text-xs text-slate-500">
        © {new Date().getFullYear()} SmartBMI — dibuat dengan ❤️
      </footer>
    </div>
  );
}

// File konfigurasi Vercel (tambahkan ke root project sebagai vercel.json)
// {
//   "version": 2,
//   "builds": [
//     { "src": "package.json", "use": "@vercel/static-build", "config": { "distDir": "dist" } }
//   ],
//   "routes": [
//     { "src": "/(.*)", "dest": "/index.html" }
//   ]
// }
