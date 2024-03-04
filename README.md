# world_population.csv & world_gdp_data.csv

# 1-ÖN İŞLEME KODLARI 🥇

 Gerekli kütüphanelerimizi import ediyoruz
 ---
 ```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


df = pd.read_csv("world_gdp_data.csv", encoding='latin-1')
```
 İlk datasetimizi yüklüyoruz 
 ---
```
df = pd.read_csv("world_gdp_data.csv", encoding='latin-1')
silinecek_sutunlar = ["country_name","indicator_name"]
```
 Boş kısımları doldurabilmek için "string" ifadeli sütunları siliyoruz 
 ---
```
df2 = df.drop(silinecek_sutunlar, axis=1)
```
 Ortamalar adlı bir tutucu tanımlıyoruz ve boş verilerimizi diğer verilerimizin ortalaması ile dolduruyoruz
 ---
```
ortalamalar = df2.mean() 
df2 = df.fillna(ortalamalar)
```
 İkinci datasetimizi yüklüyoruz
 ---
```
df1 = pd.read_csv("world_population.csv", encoding='latin-1')
```
 Birinci ve ikinci kütüphanelerimizi yatay olarak birleştiriyoruz
 ---
```
df_concat = pd.concat([df2, df1],axis=1)
```
 İlerleyen süreçte veri çekmeye çalışırken "syntax" hatası aldığımız için şimdiden kullanacağımız bazı satırların isimlerini değiştiriyoruz ve bu değişikliklerin kalıcı olması için "inplace=True" ifadesini kullanıyoruz
 ---
```
df_concat.rename(columns={"Country/Territory":"country_name"},inplace=True)
df1.rename(columns={"Country/Territory":"country_name"},inplace=True)
df_concat.rename(columns={"2000 Population":"ikibin_population"},inplace=True)
df_concat.rename(columns={"2010 Population":"ikibinon_population"},inplace=True)
df_concat.rename(columns={"Growth Rate":"Growth_Rate"},inplace=True)
```
 Ülke isimleri sütunlarını eşitledikten sonra ortak sütunları birleştirdik ve bunu "country_names" isimli dosyaya atadık
 ---
```
country_names = pd.concat([df, df1], join='inner')
```
 df adlı dataframemizde country_name sütununda tekrar eden verileri attık
 ---
```
df = df.drop_duplicates(subset=["country_name"])
```
 Merge fonksiyonu ile eşleşen sadece eşleşen ülkeleri yazmayı sağladık
---
```
df_real = pd.merge(df, df1, on='country_name')
```
 Silmemiz gereken "string" içeren sütunları tanımladık ve sonrasında o sütunları sildik sonrasında yeni oluşan data framemizi "df_real2" adlı df'ye atadık
 ---
```
silinecek_sutunlar = ["country_name","indicator_name","CCA3","Capital","Continent"]
df_real2 = df_real.drop(silinecek_sutunlar, axis=1)
```
 df_real2 dosyamızda ki boş yerlerimizi verilerimizin ortalaması ile doldurduk
 ---
```
ortalamalar = df_real2.mean()
df_real2 = df_real2.fillna(ortalamalar)
```
 df_real adlı df'mizden istemediğimiz sütunları attık ve sonrasında onları aynı isimde tutucalarla tuttuk
 ---
```
country_name = df_real.pop('country_name')
indicator_name = df_real.pop('indicator_name')
CCA3 = country_name = df_real.pop('CCA3')
Capital = df_real.pop('Capital')
Continent = df_real.pop('Continent')
```
 Tuttuğumuz sütunları df_real2 ile birleştirdik ve df'mizin son halini hazırladık
 ---
```
df_concat_son = pd.concat([df_real2, Continent, Capital, country_name, indicator_name, CCA3],axis=1)
```
 Sonrasında kullanmak istediğimiz bazı sütunların isimlerini değiştirdik 
 ---
```
df_concat_son.rename(columns={"2000 Population":"ikibin_population"},inplace=True)
df_concat_son.rename(columns={"2010 Population":"ikibinon_population"},inplace=True)
df_concat_son.rename(columns={"Growth Rate":"Growth_Rate"},inplace=True)
df_concat_son.rename(columns={"World Population Percentage":"World_Population_Percentage"},inplace=True)
```
# 2-LİSTELER 🥈
 Histogram düzeyinde kaç ülkenin o büyüme oranına sahip olduğunu belirten bir grafik oluşturduk 
 ---
```
plt.hist(df_concat_son.Growth_Rate,bins= 20) #bins histodaki cubuklarin inceligini arttirir
plt.xlabel("Buyume Orani")
plt.ylabel("Ülke Sayisi")
plt.title("hist")
plt.show()
```
<img width="597" alt="Ekran Resmi 2024-03-04 14 24 06" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/4a84735d-8ea4-4988-b4ac-f2639b90d258">

 * İçerik : Belirtilen büyüme oranına sahip kaç ülke olduğunu görebiliriz.


 Bar düzeyinde ikibinon yılında kıtaların nüfus sayılarını karşılaştırdığımız grafiği oluşturduk
 ---
```
plt.bar(df_concat_son.Continent,df_concat_son.ikibinon_population, width=0.5)
plt.ylabel("Nüfus Sayısı")
plt.xlabel("Kıta İsimleri")
plt.title("İkibinon Senesinin Nüfus Dağılımı")
plt.show()
```
<img width="601" alt="Ekran Resmi 2024-03-04 14 25 19" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/e761261a-e3ac-41d4-a6ce-ff794161dbd9">

 * İçerik : İkibinon yılında kıtaların toplam nüfus sayısını görebilir ve bu bağlamda karşılaştırmalar yapabiliriz

 Scatter düzeyinde ülkelerin ikibinon yılında büyüme oranı değerleriyle eşleştirildiği grafiği oluşturduk
---
```
plt.scatter(df_concat.Growth_Rate, df_concat.ikibinon_population, color = "blue")
plt.show()
```

<img width="574" alt="Ekran Resmi 2024-03-04 14 27 00" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/3d35bbf6-83e6-4acb-b5d7-cebaec5f6269">

 * İçerik : Ülkelerin 2010 senesinde ki nüfusu ile genel büyüme oranını karşılaştırabiliriz


 Scatter düzeyinde ikibinon yılında ülkelerin popülasyonuna bağlı olarak ekonomilerinin ne kadar geliştiği grafiğini oluşturduk
 ---
```
plt.scatter(df_concat_son.ikibinon_population, df_concat_son.ikibinon, color = "red")
plt.show()
```

<img width="610" alt="Ekran Resmi 2024-03-04 14 27 26" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/32b39052-aa51-4b6e-ada9-e9c1f6c45e3c">

 * İçerik : Ülkelerin nüfusuna bağlı olarak ekonomilerininin ne kadar büyüdüğünü karşılaştırabiliriz


 Bar düzeyinde kıtalara göre area büyüklüğünün karşılaştırılması
 ---
```
plt.bar(df_concat_son.Continent, df_concat_son.Area, width = 0.5)
plt.show()
```

<img width="628" alt="Ekran Resmi 2024-03-04 14 28 22" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/8fa7463b-bedf-489e-b3de-3dbe90e38e2b">

 * İçerik : Kıtaların toplam area büyüklüklerinin karşılaştırılması yapılabilir
 Binlere ayırma 
 ---
```
binler = np.array_split(df_concat_son, 17)

for i, binary in enumerate(binler):
    print(f"Bin {i+1}: {binary}")
```

<img width="802" alt="Ekran Resmi 2024-03-04 14 29 11" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/8daf7d33-5d84-4963-a75f-c66b51bac490">

 * İçerik : Binlerin genel görünümü


 Scatter düzeyinde ülkelerin arealarının karşılaştırılması
 ---
```
plt.scatter(binary.CCA3, binary.Area, color = "red")
plt.show()
```
<img width="1456" alt="Ekran Resmi 2024-03-04 14 30 54" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/6fa507d2-05ce-4071-9c59-f59c78d28824">

 * İçerik : Ülkelerin area karşılaştırılmasının noktalar ile yapılmasına olanak sağlarız



 Tanımladığımız binary ile turta şeklinde areaların karşılaştırılması 
 ---
```
binary['Area'].plot(kind = 'pie', figsize = (10,5), shadow = True, autopct ='%1.2f%%')
plt.title('Areaların karşılaştırılması')
plt.axis('equal')
plt.show()
```

<img width="574" alt="Ekran Resmi 2024-03-04 16 36 46" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/f82168cd-4dc6-4740-99ce-fbeed8d8e7ac">


 * İçerik : Binaryimizin içerisindeki ülkelerin arealarının karşılaştırılması sağlanır

  Tanımladığımız binary ile turta şeklinde densitylerinin karşılaştırılması
  ---
```
binary['Density'].plot(kind = 'pie', figsize = (10,5), shadow = True, autopct ='%1.2f%%')
plt.title('Areaların karşılaştırılması')
plt.axis('equal')
plt.show()
```

<img width="695" alt="Ekran Resmi 2024-03-04 16 38 12" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/6d4173ff-c54f-4652-9c17-29503eb6ef0d">


 * İçerik : Binarymizin içerisindeki ülkelerin densityleri karşılaştırılması sağlanır

 1970 ile 2022 yılları arasındaki nüfus değişimi
---
```
plt.subplots(figsize=(10,5))
trend = df_concat_son.iloc[:,47:54].sum().sort_values(ascending=True)
sns.lineplot(x=trend.index, y=trend.values, marker="o")
plt.xticks("Years")
plt.ylabel("Population")
plt.title("World Population Trend (1970-2022)")
plt.show()
```



<img width="1515" alt="Ekran Resmi 2024-03-04 16 45 25" src="https://github.com/buzzi0/w_p-_w_g_d/assets/103946477/4b5673da-5d20-4213-b04a-266cfab82186">


* İçerik : 1970 ile 2022 yılları arasında 10'ar artış ile belirlediğimiz yılların yılları içerisindeki nüfus değişimini görmemizi sağlar




