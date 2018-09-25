# saptodb
 SapToDb Dashboard, İş Zekası yada Excel gibi uygulamalara hızlı, basit ve kullanışlı şekilde data çekmenize yarar. SAP üzerinde Rfc Fonksiyonlarını veya direk tabloları kullanarak veri alabilir. 

# Kullanımı
1. saptodb.exe.config dosyası içerisinde SAP.Middleware.Connector altında SAP bağlantı bilgilerinizi tanımlayınız. Ayrıca aynı dosya içerisinde connectionStrings altına verilerinizi aktarmak istediğiniz SqlServer bağlantı bilgilerinizi giriniz.
Bu aşamadan sonra kullanıma hazırsınız.   
  Not: SapToDb uygulaması doğru kurgulandığında RFC ve BAPI fonksiyonlarını çalıştırabilir niteliktedir. Bu nedenle sisteminizde değişiklik yapılmasını istemediğiniz durumlarda yukarıdaki kullanıcının readonly olmasına dikkat edilmelidir.  
  
2. Uygulama xml dosyaları ile çalışmaktadır. Parametlerin büyük bir çoğunluğu xml dosyasından gelmektedir. Sablon*.xml dosyası incelendiğinde 5 ana gruptan oluştuğu görülür. Şablon dosyası içerisinde bölümlerin kullanım amaçları hakkında bilgi alabilirsiniz. 
Xml Dosyası Yapısı:  
   SAP üzerindeki RFC fonksiyonunu kullanabilmek için aşağıdaki bilgilerinin bilinmesi gerekir.  
   *header -> rfcname:* RFC fonksiyonunun adı.  
   header -> SapExportTableName: Veri dönderecek Export Table name.   
   SapImport: Fonksiyon çalıştırılmadan önce alacağı parametrelerin tanımı ve değeri örn:name="IERDAT" value="2018-09-21"  
   
   MapField: SAP den dönecek veri setindeki kolon isimleri ve aktarmak istediğiniz SQLServer Tablosundaki kolonların map edildiği alandır.  
   dbOperation: Verilerin veri tabanına aktarılmadan önce veya sonra çalıştırılmasını istediğiniz sql komutlarının girildiği alandır.  
   


## Help

#### Usage: SapToDb [xmlfilename] [options] <-dynamickey=dynamicvalue...n> 

#### Arguments:
    -file=FileName.xml : xmlFileName :Aktarım yapılacak RFC fonksiyonu ve SqlServer tablosu bilgileri, map tablosu ve detayları buradan belirlenir.
    -dynamickey=dynamicvalue : bu bilgiler xml dosyası içerisinde import parametreleri arasında dynamicvalue değeri
                                true olduğunda anlamlı olur. Eğer dynamicvalue= tru ise aynı import parametresinin adı 
                                key değeri olacaktır. Argumanlarda belirttiğiniz value değeri ise value olarak kullanılacaktır.
                Örnek: Çağrılan bir SAP Foksiyonu 2 adet değer almaktadır. 1. Değer Başlangıç Tarihi(ERDAT) 2. Değer Bitiş Tarihi(SERDAT). 
                       Bu başlangıç ve bitiş tarihlerini dinamik olarak set etmek için. 
        Saptodb -file=satislar.xml -ERDAT=2018-09-22 -SERDAT=2018-09-24
        Not: dynamickey=dynamicvalue değer çifti sonsuz miktarda veri içerebilir. xml belgesindeki alan kadar veri olmalıdır.Aksi durumda
             xml belgesindeki default veri geçerli olacaktır.
          
#### Options:
    -outputtype = <çıktı_türü> : SAP den alınan verinin hangi çıktı türüne gönderileceğini belirler.
                                    varsayılan olarak 'screen' seçilidir. 
                    çıktı_türleri : screen, xmlfile, database 
                    Önemli Not: outputtype özelliği xmlveri şaplonu header tagından da etkilenmektedir.
                                Öncelik sırası olarak, önce komut satırına eğer boşsa xml belgesine oda boşsa
                                varsayılan değer geçerli olur. 
    -textfilename = eğer outputtype='xmlfile' seçildi ise oluşturulacak xml dosyasının adı burada belirtilir. Boş bırakılırsa 
              önce xml dosyası header texfilename alanına bakılır orasıda boş ise varsayılan olarak günün tarihi eklenir. 
                Not: eğer aynı isimde dosya varsa üzerine yazılacaktır.
    -gettablestructure=<SAP_TABLE_NAME> SAP de yer alan bir tablonun yapısını getirir.
                   Örnek : saptodb -gettablestructure=MARA -outputtype=xmlfile -textfilename=MARA_Structure
    

Use 'SapToDb --help' for more information about a command.
This is SAP to Database or xml file data convertor application.
