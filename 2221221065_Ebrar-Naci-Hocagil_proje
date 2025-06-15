# Gerekli kütüphaneleri içe aktar
import math
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Veri setini oku ve eksik değerleri çıkar
dataset = pd.read_csv("top-rated-coffee-clean.csv")
scores = dataset["total_score"].dropna()

# Temel istatistikleri hesapla
ortalama = np.mean(scores)
medyan = np.median(scores)
std = np.std(scores, ddof=1) 
varyans = np.var(scores, ddof=1)
n = len(scores)
stdhata = std / np.sqrt(n)  

# Temel değerleri yazdır
print("Ortalama:", round(ortalama, 4))
print("Medyan:", round(medyan, 4))
print("Standart Sapma:", round(std, 4))
print("Varyans:", round(varyans, 4))
print("Standart Hata:", round(stdhata, 4))

# Histogram grafiği 
plt.hist(scores, bins=20, color="lightgreen", edgecolor="black")
plt.title("Histogram")
plt.xlabel("Puan")
plt.ylabel("Frekans")
plt.show()

# Boxplot grafiği 
plt.boxplot(scores, vert=False)
plt.title("Boxplot")
plt.xlabel("Puan")
plt.show()

# Ortalama için %95 güven aralığı 
dfree = n - 1
z = 1.96 
t_crit = z + (z**3 + z) / (4 * dfree)
alt_sinir = ortalama - t_crit * stdhata
ust_sinir = ortalama + t_crit * stdhata
print("Ortalama için %95 güven aralığı:", 
      "({:.2f}, {:.2f})".format(alt_sinir, ust_sinir))

# varyan için %95 güven aralığı
dfree = n - 1
z_upper = -1.96   # 1 - alpha/2 → 0.975 noktasının z-skoru = -z_{0.025}
z_lower = 1.96    # alpha/2 → 0.025 noktasının z-skoru =  z_{0.975}
chi2_alt = dfree * (1 - 2/(9*dfree) + z_upper * math.sqrt(2/(9*dfree)))**3
chi2_ust = dfree * (1 - 2/(9*dfree) + z_lower * math.sqrt(2/(9*dfree)))**3
var_lower = dfree * varyans / chi2_ust
var_upper = dfree * varyans / chi2_alt
print(f"Varyans için %95 güven aralığı (yaklaşık): ({var_lower:.4f}, {var_upper:.4f})")
# Aykırı değerleri bul (IQR yöntemine göre)
q1 = scores.quantile(0.25)
q3 = scores.quantile(0.75)
iqr = q3 - q1
alt_sinir = q1 - 1.5 * iqr
ust_sinir = q3 + 1.5 * iqr
aykirilar = scores[(scores < alt_sinir) | (scores > ust_sinir)]
print("Aykırı değer sayısı:", len(aykirilar))

# Hipotez testi (ortalama 94 mü değil mi diye bakıyoruz)
mu0 = 94
t_stat = (ortalama - mu0) / stdhata
print("t-istatistiği (manuel):", round(t_stat, 4))

# Yaklaşık tablo değeri 2 olarak alınmış
if abs(t_stat) > 2.0:
    print("H0 reddedildi.")  # ortalama 94'ten farklı olabilir
else:
    print("H0 reddedilemedi.")  # ortalama 94 olabilir

# Örneklem büyüklüğü (±0.1 hata ve %90 güven için)
z = 1.645
n_gerekli = (z * std / 0.1) ** 2
print("Gerekli örneklem büyüklüğü:", round(n_gerekli))
