# Erzurum-hacim-araci
Erzurum iklimine uygun hacimsel yapı algoritmaları
import streamlit as st
import math

st.set_page_config(page_title="Erzurum Hacimsel Yapı Öneri Aracı", layout="centered")

st.title("🏔️ Erzurum’a Uygun Hacimsel Yapı Öneri Aracı")
st.markdown("Bu araç, Erzurum’un iklimsel koşullarına uygun yapı hacmi önerileri üretir.")

# Kullanıcı girdileri
parsel_alan = st.number_input("Parsel Alanı (m²)", min_value=50.0, step=10.0)
yapilabilir_alan = st.number_input("Toplam Yapı Alanı (m²)", min_value=50.0, step=10.0)

# Erzurum’a uygun oranlar
maks_kat_sayisi = 4
min_kat_yuksekligi = 2.7
maks_kat_yuksekligi = 3.3
maks_yogunluk_orani = 0.4  # yapı alanı / parsel alanı
maks_tabana_yayilma_orani = 0.35

if parsel_alan > 0 and yapilabilir_alan > 0:

    st.subheader("📐 Önerilen Hacimsel Alternatifler")

    alternatifler = []

    for kat_sayisi in range(1, maks_kat_sayisi + 1):
        kat_alani = yapilabilir_alan / kat_sayisi
        taban_oran = kat_alani / parsel_alan

        if taban_oran <= maks_tabana_yayilma_orani:
            for kat_yuksekligi in [min_kat_yuksekligi, (min_kat_yuksekligi + maks_kat_yuksekligi)/2, maks_kat_yuksekligi]:
                toplam_yukseklik = kat_sayisi * kat_yuksekligi
                hacim = yapilabilir_alan * kat_yuksekligi
                alternatifler.append({
                    "Kat Sayısı": kat_sayisi,
                    "Kat Yüksekliği (m)": round(kat_yuksekligi, 2),
                    "Toplam Yükseklik (m)": round(toplam_yukseklik, 2),
                    "Taban Alanı (m²)": round(kat_alani, 2),
                    "Hacim (m³)": round(hacim, 2)
                })

    for i, alt in enumerate(alternatifler):
        with st.expander(f"Alternatif {i+1}"):
            st.write(f"- **Kat Sayısı:** {alt['Kat Sayısı']}")
            st.write(f"- **Kat Yüksekliği:** {alt['Kat Yüksekliği (m)']} m")
            st.write(f"- **Toplam Yükseklik:** {alt['Toplam Yükseklik (m)']} m")
            st.write(f"- **Taban Alanı:** {alt['Taban Alanı (m²)']} m²")
            st.write(f"- **Yapı Hacmi:** {alt['Hacim (m³)']} m³")
else:
    st.info("Lütfen geçerli bir parsel alanı ve yapı alanı giriniz.")
