## C# Extension Metod Kullanımı

Kelime anlamı genişletilebilir metod olan Extension Method'lar C#3.0 ile hayatımıza girmiştir ve yaptığı iş itibatiyle kullanım açısından son derece faydalı metodlardır. Tek cümleyle özetlemek gerekirse class ve struct yapılarını modify etmeden ilgili struct yada class'için extension metodlar eklememizi sağlar.

 Extesion metod yazarken uymamız gereken bir kaç kural vardır. Bunlar;

+ Extension metodlar **static** bir class içerisinde static olarak tanımlanmalıdır. 
+ Extend edilecek class ilgili extension metoda metodun ilk parametresi olarak verilip önünde **this** keyword'ü ile tanımlanmalıdır
+ Extension metod da tanımlı parametrelerden sadece 1 tanesi **this** keyword'ü ile tanımlanır

Hemen bir örnek üzerinde inceleyelim. Case'imiz şu olsun; Bir tane extension metodumuz var ve bu metod **integer** bir değer alıp asal mı değil mi diye kontrol etsin.

```
public static class MyExtensions
   {
       public static bool IsPrime(this int integer)
       {
           //tembellik edip implementation'ı yazmakla uğraşmadım :)
           return true;
       }
   }

```
*Yazdığımız metodu aşağıdaki görselde olduğu gibi kullanmak istediğimizde int tanımlı değişkenin adı"." nokta dediğimizde extensionMetod'u intellisense de görebileceğiz.*


![extensionmethoddemo](https://user-images.githubusercontent.com/61011022/85738769-f23e1e00-b708-11ea-8447-908c2247e2b3.jpg)

```
class Program
{
    static void Main(string[] args)
    {
        int anyNumber = 123456;
        if(anyNumber.IsPrime())
        {
            //asal sayı
        }
        else
        {
            //asal sayı değil
        }
    }
}
```
Heralde en güzel yanı da bu olsa gerek metodu extension tanımladığımız için sanki o metod int struct'ına içerisinde tanımlı bir metodmuş gibi direkt olarak "." nokta deyip kullanabiliyoruz. 

Yukarıda ki örnekte **int** tipini baz alarak ilerledim ancak ihtiyaç dahilinde bütün tipler ve kendi tanımladığımız objeler içinde extension metodlar yazabiliriz.

Yine örnek olarak ; bir Person class'ımız ver ve içerisinde DateTime tipinde ismi BirthDate olan bir property olsun. Bir tane GetBirthDate adında extension metod tanımlayalım ve bu metod bize parametre olarak aldığı Person objesinde bulunan BirthDate alanını return ettirsin.


```
public class Person
{
    public string FullName {get;set;}
    public DateTime BirthDate {get;set;}
}
 
public static class MyExtensions
{
    public static DateTime GetBirthDate(this Person prs)
    {
            return prs.BirthDate;
    }
}

```
Şimdi bu metodu kullanan kısmı yazalım

```
using System;
                     
class Program
{
    static void Main(string[] args)
    {
        Person prs=new Person();
        DateTime bDAte = prs.GetBirthDate();
    }
}

```
Görüldüğü gibi Extension metod kullanım alanımızı ihtiyaca göre genişletip metodlarımızı istediğimiz yerde "." nokta diyerek çağırabiliyoruz.

### *C# Extension Metod ile ilgili daha ayrıntılı bilgi  ve örnekler için aşağıdaki kaynakları inceleyebilirsiniz:*
 + https://docs.microsoft.com/tr-tr/dotnet/csharp/programming-guide/classes-and-structs/extension-methods
 + https://www.tutorialsteacher.com/csharp/csharp-extension-method


## Kaynakça
+ http://www.canertosuner.com/post/C-Extension-Method-Kullan%C4%B1m%C4%B1
