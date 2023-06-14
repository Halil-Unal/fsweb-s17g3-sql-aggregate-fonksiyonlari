# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
select ograd,ogrsoyad,atarih from ogrenci inner join islem on ogrenci.ogrno=islem.ogrno
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	select k.kitapadi,t.turadi from kitap as k inner join tur as t on k.turno=t.turno where turadi='Fıkra' or turadi='Hikaye'

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

	select o.ograd,o.ogrsoyad,o.ogrno,i.kitapno from ogrenci as o inner join islem as i on i.ogrno=o.ogrno
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	select ograd,ogrsoyad,atarih from ogrenci inner join islem on ogrenci.ogrno=islem.ogrno
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	select k.kitapadi,t.turadi from kitap as k inner join tur as t on k.turno=t.turno where turadi='Fıkra' or turadi='Hikaye'
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
	select o.ograd,o.ogrsoyad,o.ogrno,k.kitapadi from ogrenci as o inner join islem as i on i.ogrno=o.ogrno inner join kitap as k on k.kitapno=i.kitapno order by ograd

	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

	select ograd,ogrsoyad,atarih from ogrenci left join islem on ogrenci.ogrno=islem.ogrno
	
	8) Kitap almayan öğrencileri listeleyin.
	
		select ograd,ogrsoyad,atarih from ogrenci left join islem on ogrenci.ogrno=islem.ogrno where atarih is null
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
	select k.kitapno,k.kitapadi,count(k.kitapadi) from kitap as k inner join islem as i on i.kitapno=k.kitapno group by k.kitapadi,k.kitapno order by kitapno

	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

select k.kitapno,k.kitapadi,count(k.kitapadi) from kitap as k left join islem as i on i.kitapno=k.kitapno group by k.kitapadi,k.kitapno order by kitapno

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
	select o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o inner join islem as i on i.ogrno=o.ogrno inner join kitap as k on k.kitapno=i.kitapno order by ograd
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
	select o.ograd,o.ogrsoyad,k.kitapadi,y.yazarad,y.yazarsoyad,i.atarih,t.turadi from ogrenci as o inner left join islem as i on i.ogrno=o.ogrno inner join kitap as k on k.kitapno=i.kitapno inner join yazar as y on y.yazarno=k.yazarno inner join tur as t on t.turno=k.turno  order by ograd
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
	select o.ograd,count(i.kitapno) as toplam_sayi from islem as i left join ogrenci as o  on o.ogrno=i.ogrno where o.sinif='10A' or o.sinif='10B' group by o.ograd 
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

	select avg(sayfasayisi) from kitap 
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
	SELECT kitapadi FROM kitap WHERE sayfasayisi > (SELECT AVG(sayfasayisi) FROM kitap);
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
	select count(ogrno) from ogrenci

	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	select count(ogrno) as 'toplam sayı' from ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
SELECT COUNT(DISTINCT ograd) AS toplam_sayi FROM ogrenci;
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	select max(sayfasayisi) from kitap

	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	select kitapadi,sayfasayisi from kitap where sayfasayisi=(select max(sayfasayisi) from kitap)
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	select min(sayfasayisi) from kitap
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	select kitapadi,sayfasayisi from kitap where sayfasayisi=(select min(sayfasayisi) from kitap)
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

		SELECT k.sayfasayisi
FROM kitap AS k
INNER JOIN tur AS t ON t.turno = k.turno
WHERE t.turadi = 'Dram'
AND k.sayfasayisi = (
  SELECT MAX(kitap.sayfasayisi)
  FROM kitap
  INNER JOIN tur ON tur.turno = kitap.turno
  WHERE tur.turadi = 'Dram'
);

	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	

	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	select count(ograd),ograd from ogrenci group by ograd
	 26) Her sınıftaki öğrenci sayısını bulunuz.
	
	select count(ogrno),sinif from ogrenci group by sinif
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
	select count(ogrno),sinif from ogrenci no where cinsiyet='K' GROUP BY sinif
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
SELECT sum(k.sayfasayisi),o.ograd,o.ogrsoyad FROM ogrenci as o inner join islem as i on i.ogrno=o.ogrno inner join kitap as k on k.kitapno=i.kitapno group by ograd,ogrsoyad order by  sum(k.sayfasayisi) desc
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.