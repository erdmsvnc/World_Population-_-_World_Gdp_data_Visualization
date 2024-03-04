# w_p-_w_g_d

#Gerekli kütüphanelerimizi import ediyoruz
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

#İlk datasetimizi yüklüyoruz 

df = pd.read_csv("world_gdp_data.csv", encoding='latin-1')
silinecek_sutunlar = ["country_name","indicator_name"]

#Boş kısımları doldurabilmek için "string" ifadeli sütunları siliyoruz 

df2 = df.drop(silinecek_sutunlar, axis=1)

#Ortamalar adlı bir tutucu tanımlıyoruz ve boş verilerimizi diğer verilerimizin ortalaması ile dolduruyoruz

ortalamalar = df2.mean() 
df2 = df.fillna(ortalamalar)

#İkinci datasetimizi yüklüyoruz

df1 = pd.read_csv("world_population.csv", encoding='latin-1')

#Birinci ve ikinci kütüphanelerimizi yatay olarak birleştiriyoruz

df_concat = pd.concat([df2, df1],axis=1)

#İlerleyen süreçte veri çekmeye çalışırken "syntax" hatası aldığımız için şimdiden kullanacağımız bazı satırların isimlerini değiştiriyoruz ve bu değişikliklerin kalıcı olması için "inplace=True" ifadesini kullanıyoruz

df_concat.rename(columns={"Country/Territory":"country_name"},inplace=True)
df1.rename(columns={"Country/Territory":"country_name"},inplace=True)
df_concat.rename(columns={"2000 Population":"ikibin_population"},inplace=True)
df_concat.rename(columns={"2010 Population":"ikibinon_population"},inplace=True)
df_concat.rename(columns={"Growth Rate":"Growth_Rate"},inplace=True)

#Ülke isimleri sütunlarını eşitledikten sonra ortak sütunları birleştirdik ve bunu "country_names" isimli dosyaya atadık

country_names = pd.concat([df, df1], join='inner')

#df adlı dataframemizde country_name sütununda tekrar eden verileri attık

df = df.drop_duplicates(subset=["country_name"])

#Merge fonksiyonu ile eşleşen sadece eşleşen ülkeleri yazmayı sağladık

df_real = pd.merge(df, df1, on='country_name')

#Silmemiz gereken "string" içeren sütunları tanımladık ve sonrasında o sütunları sildik sonrasında yeni oluşan data framemizi "df_real2" adlı df'ye atadık

silinecek_sutunlar = ["country_name","indicator_name","CCA3","Capital","Continent"]
df_real2 = df_real.drop(silinecek_sutunlar, axis=1)

#df_real2 dosyamızda ki boş yerlerimizi verilerimizin ortalaması ile doldurduk

ortalamalar = df_real2.mean()
df_real2 = df_real2.fillna(ortalamalar)

#df_real adlı df'mizden istemediğimiz sütunları attık ve sonrasında onları aynı isimde tutucalarla tuttuk

country_name = df_real.pop('country_name')
indicator_name = df_real.pop('indicator_name')
CCA3 = country_name = df_real.pop('CCA3')
Capital = df_real.pop('Capital')
Continent = df_real.pop('Continent')

#Tuttuğumuz sütunları df_real2 ile birleştirdik ve df'mizin son halini hazırladık

df_concat_son = pd.concat([df_real2, Continent, Capital, country_name, indicator_name, CCA3],axis=1)

#Sonrasında kullanmak istediğimiz bazı sütunların isimlerini değiştirdik 

df_concat_son.rename(columns={"2000 Population":"ikibin_population"},inplace=True)
df_concat_son.rename(columns={"2010 Population":"ikibinon_population"},inplace=True)
df_concat_son.rename(columns={"Growth Rate":"Growth_Rate"},inplace=True)
df_concat_son.rename(columns={"World Population Percentage":"World_Population_Percentage"},inplace=True)

#Histogram düzeyinde kaç ülkenin o büyüme oranına sahip olduğunu belirten bir grafik oluşturduk 

plt.hist(df_concat_son.Growth_Rate,bins= 20) #bins histodaki cubuklarin inceligini arttirir
plt.xlabel("Buyume Orani")
plt.ylabel("Ülke Sayisi")
plt.title("hist")
plt.show()

#Bar düzeyinde ikibinon yılında kıtaların nüfus sayılarını karşılaştırdığımız grafiği oluşturduk

plt.bar(df_concat_son.Continent,df_concat_son.ikibinon_population, width=0.5)
plt.ylabel("Nüfus Sayısı")
plt.xlabel("Kıta İsimleri")
plt.title("İkibinon Senesinin Nüfus Dağılımı")
plt.show()

#Plot düzeyinde ikibin popülasyonun kıtalara göre değişiminin grafiğini oluşturduk

plt.plot(df_concat.Continent, df_concat.ikibin_population, color = "blue")
plt.show()

#Scatter düzeyinde ülkelerin ikibinon yılında büyüme oranı değerleriyle eşleştirildiği grafiği oluşturduk

plt.scatter(df_concat.Growth_Rate, df_concat.ikibinon_population, color = "blue")
plt.show()

