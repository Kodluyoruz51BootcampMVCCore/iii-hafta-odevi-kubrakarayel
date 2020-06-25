## C# / Async programlama ve Thread Kavramı 

Async programlama bölümüne geçmeden Thread ve multithread yapılarından bahsetmek istiyorum. Thread yapısından bahsetmeye başlayarak yazıya başlayalım. Multithreading, thread scheduler tarafından yönetilir. CLR tarafından OS’ye delegate edilen bir fonksiyondur. Scheduler, her thread yapısının makul bir execution süresi içerisinde tamamlanacağını garanti eder. Bekleyen veya blocklanan threadler CPU zamanını harcamazlar.

Tek işlemcili bilgisayarlarda, thread scheduler hazırda çalışmak için bekleyen thread yapılarını belirli scheduling algoritmalarına göre işleme almaktadır. Her seferinde tek thread çalışmaktadır. Çok işlemcili bilgisayarlarda ise, birden fazla thread aynı anda çalışabilmektedir. Bu sayede scheduling algoritmaları ile daha efektif işlem süreleri içerisinde işlemleri tamamlamaktayız.


## Thread vs Process

Thread yapıları işletim sistemi işlemlerine benzemektedir. OS içerisinde işlemler birbirine paralel olarak çalışmaktadır. Thread yapısında ise, threadler bir process içerisinde birbirlerine paralel olarak çalışmaktadırlar. İşlemler birbirlerine tamamen izoledir. Threadler birbirlerine tamamen izole değillerdir. Aynı hafızayı paylaşırlar bundan dolayı birlikte çalışan threadler arasından veriler rahatça paylaşılmaktadır.

![process-thread](https://user-images.githubusercontent.com/61011022/85775992-f5e19d00-b728-11ea-9938-e222a626568b.png)

Thread ve Task yapısının kullanım örneklerine geçmeden önce bazı kavramları netleştirelim. Sıklıkla kullanılan bir örnek üzerinden anlatacağım. Restaurantta yumurta ve tost siparişi geldiğini düşünelim.

+ **Synchronous:** Önce yumurtayı pişir. Ardından tostu hazırla.

![singlethreaded](https://user-images.githubusercontent.com/61011022/85776193-1f9ac400-b729-11ea-80a9-ef6a7459e986.png)

                                         sync single thread
                                            
                                            
![multithreaded](https://user-images.githubusercontent.com/61011022/85776359-4eb13580-b729-11ea-9c11-46c06be13822.png)

                             sync multithread
                                             
                                             
+ **Asynchronous – single threaded:** Yumurtayı mutfakta pişirmeye başla bekle. Ardından tostu makinede hazırlamaya başla ve bekle. Bekleme esnasında yeni sipariş gelirse al. Pişirme süreleri bitince müşterilere servis yap. 


![async-single](https://user-images.githubusercontent.com/61011022/85776543-7e603d80-b729-11ea-9a91-cdf167f6b668.png)

                                           async single thread
                                            
 
+ **Asynchronous –  multithreaded:** 2 yeni aşçı işe alalım. Biri sadece yumurta pişirsin. Diğeri ise sadece tost hazırlama işini halletsin. Mutfakta malzeme kullanırken birbirleri çakışmalarını engellememiz lazım. Ayrıca her ikisinede maaş ödemek durumundayız.


![async-mutlithreaded](https://user-images.githubusercontent.com/61011022/85777052-f2024a80-b729-11ea-99ac-25fdfae92991.png)

                                           async multithread
                                             
Async programlama ile sistemin esnekliği ön plana çıkar. Örneğin; uzun bir hesaplama yapılacağı zaman async programlama ile yapılırsa arka plan’da çalışan thread içerisinde bu işlem halledilir. Sistem ön planda hala esnek durumda kalacaktır.

**Paralel programlama** ise farklıdır. Her ne kadara bu yazıda bahsetmeyecek olsak bile bahsetmekte fayda olduğunu düşünüyorum.  Paralel programlama ile sistemin CPU performansı gibi özellikler ön plandadır ve tüm çalışan thread yapıları tek bir işlemi yapmak için çalışır. Belirli bir task’ı birden fazla thread kullanacak hızlıca tamamlamaktır. Task’ı tamamlamak için birden fazla kaynak kullanmak olarak düşünebiliriz. 


## Thread
Teorik girişten sonra artık console application üzerinde threadler ile ilgili işlemlere başlayalım. Aşağıdaki örnekte WriteY() adlı metod thread tanımlanarak çağrılmıştır. Console uygulamasının çalıştığı main metodu içerisinde i  değişkeni 0’dan 100′ e kadar giden for içerisinde “x” ekrana yazılacak. WriteY içinde ise aynı şekilde for döngüsü içerisinde y yazılacaktır.


![thread01](https://user-images.githubusercontent.com/61011022/85777551-65a45780-b72a-11ea-94e7-a73e5c14e6ab.png)



Program çıktısı her seferinde farklı olacaktır. Çünkü thread schedular tarafından düzenlenen çalışma sırası her seferinde farklı çalışma öncelikleri ile threadleri tamamlayacaktır.


![thread01output](https://user-images.githubusercontent.com/61011022/85777669-7c4aae80-b72a-11ea-88b9-153def0d2bcd.png)

![thread01output2](https://user-images.githubusercontent.com/61011022/85777721-8b316100-b72a-11ea-87bd-9fd319ee4a21.png)

Lambda ifadeler kullanarak daha kısa thread tanımları da yapabiliriz.

![thread01lambda](https://user-images.githubusercontent.com/61011022/85777784-9b494080-b72a-11ea-82e9-4a67c619ec60.png)


Threadler ile çağırdığımız metodlara parametrede gönderebiliyoruz.

![thread02](https://user-images.githubusercontent.com/61011022/85777872-ae5c1080-b72a-11ea-98fc-7e62f4f8e6bf.png)


**Thread.CurrentThread.Name** özelliği ile çalışan thread’in ismini öğrenebiliriz. Thread ismi ayarlamak için **threadName.name = “threadismi”** şeklinde tanımlama yaparız.

Oluşturduğum tüm threadler default olarak “foreground (ön plan)” şeklinde tanımlanır. Uygulama tüm foreground threadler tamamlanınca biter. Thread özelliği “background (arka plan)”  olarak değiştirilirse, background thread devam etse bile uygulama bitebilir. Bu durumda background thread aniden kesilmiş olacaktır. **threadName.Isbackground = true** biçiminde tanımlama yapılarak özellik değiştirilebilir. **threadName.Join()** metodunu çağırarak tüm threadlerin bitmesini bekleyip programı öyle sonlandırabiliriz.

```
Thread.Sleep (TimeSpan.FromHours (1));  // 1 saat bekle

Thread.Sleep (5000);                     // 5 saniye bekle

```

Threadlerin Sleep veya Join durumlarında beklemeleri esnasında thread blocklu durumdadır ve CPU kaynaklarını harcamaz.

Aşağıdaki örnekte _field değişkeni ThreadStatic olarak tanımlanmıştır. Bu tanımlama ile _field değişkeni her thread için farklı olacaktır. Şöyle düşünelim ilk thread içerisinde  1’den 10’a kadar döngü içerisinde artıp ekrana çıktı olarak gösterilecektir. Diğer thread başladığı zaman o thread için _field değeri 0 olduğundan 0’dan 10’a kadar tekrardan artış olacaktır. ThreadStatic özelliği eklenmemiş olsa diğer thread için artış 10’dan başlayıp 20’ye kadar olacaktı.


![thread022](https://user-images.githubusercontent.com/61011022/85778397-32ae9380-b72b-11ea-8e5a-c28415c91f16.png)



*Ekran görüntüsü*


![thread021](https://user-images.githubusercontent.com/61011022/85778475-4659fa00-b72b-11ea-8a23-15cfc5f9bb5d.png)

Thread pooling yapısı ile performans açısından thread yapılarını daha verimli kullanabiliriz.

## Tasks
Async programlama ile daha sıklıkla kullanılan Task yapısı thread yapısına göre üst seviyede. Task yapısını kullanarak daha gelişmiş işlemler yapabiliriz. Thread pooling yapısını otomatik olarak kullanıp birbiri ardına eklenebilecek olan işlemleri daha iyi organize etmektedir.

Aşağıdaki yapıda tasks adında türü task olan bir dizi oluşturduk. Bu dizi 3 task değeri alacaktır. Task tanımlamalarımızı yaptıktan sonra Task.WaitAll(tasks) metodu ile tüm taskların çalışıp biteceğini garanti ettik. Aynı thread yapısında bulunan join() metodu gibi

![task01](https://user-images.githubusercontent.com/61011022/85778570-5ffb4180-b72b-11ea-9062-e185749c101a.png)

## Kaynakça
+ https://ekinyucell.wordpress.com/2016/10/14/c-async-programlama-ve-thread-kavrami-1/




