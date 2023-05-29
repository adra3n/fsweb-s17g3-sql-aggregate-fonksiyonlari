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
	
		select o.ograd Adi, o.ogrsoyad Soyadi, i.atarih AlımTarihi
		from ogrenci o, islem i
		where o.ogrno = i.ogrno
		order by atarih;
		
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
		select k.kitapadi KitapAdi, t.turadi Turu
		from kitap k, tur t
		where k.turno = t.turno
		
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	
	  	select o.ograd Adi, o.ogrsoyad Soyadi, k.kitapadi KitapAdi
		from ogrenci o, islem i, kitap k
		where o.ogrno = i.ogrno and (o.sinif = "10B" or o.sinif ="10C")
		
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
		
		select o.ograd Adi, o.ogrsoyad Soyadi, atarih AlımTarihi
		from ogrenci o
		join islem on ogrenci.ogrno = islem.ogrno
		order by ogrenci.ogrno;
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
		select k.kitapadi KitapAdi, t.turadi Tur
		from kitap k
		join tur t on t.turno = k.turno
		where t.turadi = "Fıkra" or t.turadi= "Hikaye"
		
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
		select o.ogrno No, o.ograd Adi, o.ogrsoyad Soyadi, k.kitapadi KitapAdi
		from ogrenci o
		join islem i on o.ogrno = i.ogrno
		join kitap k on i.kitapno = i.kitapno
		where o.sinif in ("10a", "10b")
        order by Adi
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
		
	 	select o.ograd Adi, o.ogrsoyad Soyadi, i.atarih AlımTarihi
		from ogrenci o
		left join islem i on o.ogrno = i.ogrno
        order by o.ogrno
			
	8) Kitap almayan öğrencileri listeleyin.
	
		select o.ograd Adi, o.ogrsoyad Soyadi, i.atarih AlımTarihi
		from ogrenci o
		left join islem i on o.ogrno = i.ogrno
		where o.ogrno not in (select ogrno from islem);
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
		
		select k.kitapno KitapNo, k.kitapadi Adi, count(i.kitapno) AlimSayisi
		from kitap k
		join islem i on k.kitapno = i.kitapno
		group by k.kitapno
		order by k.kitapno;
		
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
		
		select k.kitapno KitapNo, k.kitapadi Adi, coalesce(count(i.kitapno), 0) AlimSayisi
		from kitap k
		left join islem i on k.kitapno = i.kitapno
		group by k.kitapno
		order by k.kitapno;


	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
		select o.ograd Adi, o.ogrsoyad Soyadi, k.kitapadi KitapAdi
		from ogrenci o
		join islem i on o.ogrno = i.ogrno
		join kitap k on k.kitapno = i.kitapno
		
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
		select o.ograd Adi, o.ogrsoyad Soyadi, k.kitapadi KitapAdi, y.yazarad YazarAdi, t.turadi Tur, i.atarih AlimTarihi
		from ogrenci o
		left join islem i on o.ogrno = i.ogrno
		left join kitap k on k.kitapno = i.kitapno
		left join tur t on t.turno = k.turno
		left join yazar y on y.yazarno = k.yazarno
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
		
		select o.ogrno no, o.ograd adi, o.ogrsoyad soyadi, coalesce(count(i.kitapno), 0) as okudugukitapsayisi
		from ogrenci o
		left join islem i on o.ogrno = i.ogrno
		where o.sinif in ("10a", "10b")
		group by o.ogrno
		order by o.ogrno;
		
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
		
		select AVG(sayfasayisi) as OrtalamaSayfaSayisi
		from kitap;
		
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
		select * from kitap
		where sayfasayisi > (select AVG(sayfasayisi) as OrtalamaSayfaSayisi
		from kitap)
		order by sayfasayisi
	
		
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
		select count(*) OgrenciSayisi 
		from ogrenci
	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
		select count(*) "toplam sayı" 
		from ogrenci
	
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
		select count(distinct(ograd)) isimler
		from ogrenci
	
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
		
		select sayfasayisi 
		from kitap
		order by sayfasayisi desc 
		limit 1
	
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		select kitapadi KitapAdi, sayfasayisi Sayfa
		from kitap
		order by sayfasayisi desc 
		limit 1
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
		
		select sayfasayisi 
		from kitap
		order by sayfasayisi asc 
		limit 1
	
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		select kitapadi KitapAdi, sayfasayisi Sayfa
		from kitap
		order by sayfasayisi asc 
		limit 1
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
		
		select sayfasayisi 
		from kitap, tur
		where turadi = "Dram"
		order by sayfasayisi desc 
		limit 1
	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
		select SUM(sayfasayisi) "Okunan Sayfa Sayisi"
		from kitap, islem
		where ogrno = 15
		
	
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
			
		select ograd OgrenciIsmi, count(ograd) IsimSayisi 
		from ogrenci
		group by ograd


	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
		select o.sinif Sinif, count(o.sinif) OgrenciSayisi
		from ogrenci o
		group by o.sinif
		
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
		select o.sinif, count(o.ogrno) 
		from ogrenci o
		group by o.sinif, o.cinsiyet
	
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
		select o.ograd Adi,
		o.ogrsoyad Soyadi,
		SUM(k.sayfasayisi) ToplamSayfaSayisi
		from ogrenci o
		left join islem i on i.ogrno = o.ogrno
		left join kitap k on k.kitapno = i.kitapno
		group by o.ogrno
		order by ToplamSayfaSayisi desc
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
		
		select o.ogrno "Öğrenci No", 
		CONCAT(o.ograd, o.ogrsoyad) "Öğrenci" , 
		count(i.kitapno) "Okunan Kitap Sayısı" 
		from ogrenci o
		left join islem i on i.ogrno = o.ogrno
		group by o.ogrno
	
		