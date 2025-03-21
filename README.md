[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/7TXVPuTD)

<h2>1 – Neden OOP Kullanırız? Önemli OOP Dilleri Nelerdir?</h2>
<p>
Nesne Yönelimli Programlama (OOP), gerçek dünyadaki varlıkları sınıf ve nesne mantığıyla modelleyerek kodun okunabilirliğini, bakımı ve yeniden kullanılabilirliğini artırır. E-ticaret uygulamalarında, örneğin <strong>Product</strong> (ürün), <strong>Order</strong> (sipariş) veya <strong>Customer</strong> (müşteri) gibi varlıkları doğrudan nesne olarak temsil etmek daha mantıklı ve organize bir yapı sunar. Bu sayede, ürün ekleme, stok güncelleme veya sipariş oluşturma gibi işlemleri aynı konsept altında yönetebiliriz.
</p>
<ul>
    <li><strong>Önemli OOP Dilleri:</strong> Java, C#, C++, Python, vb.</li>
</ul>

<h2>2 – Interface vs Abstract Class</h2>
<p>
E-ticaret sistemlerinde ödeme (<em>payment</em>) ve kargo (<em>shipping</em>) gibi farklı operasyonlar düşünün. Sizin “ödeme” yapma işlemini <em>nasıl</em> yapacağınızı henüz bilmeseniz de <em>neyi</em> yapacağınızı (parayı çekme, onaylama vs.) belirlemek istiyorsanız, <strong>Interface</strong> size “sözleşme” sunar. “Kredi kartı ödemesi” veya “Banka kartı ödemesi” gibi sınıflar, bu interface’i uygulayarak farklı ödeme yöntemleri oluşturabilir.
</p>

<p><strong>Interface:</strong></p>
<ul>
    <li>Metotların <em>ne yapacağını</em> söyler (genelde gövdesiz).</li>
    <li>Bir sınıf birden fazla interface’i <code>implement</code> edebilir.</li>
    <li>Java 8’den itibaren “default” ve “static” metotlar ile gövdeli metotlar da eklenebilir.</li>
</ul>

<p><strong>Abstract Class:</strong></p>
<ul>
    <li>Hem soyut (<code>abstract</code>) hem de gövdeli metotlar içerebilir.</li>
    <li>Sadece <em>tek</em> bir abstract class genişletilebilir (<code>extends</code>).</li>
    <li>Ortak özellikleri, constructor’ları ve alanları tutarak alt sınıflar için bir iskelet sunar.</li>
</ul>

<p>
Kısaca, benzer davranışları paylaşmak isteniyorsa <strong>Abstract Class</strong>, farklı türde ama benzer arayüzlere sahip işlemler için ise <strong>Interface</strong> kullanılabilir.
</p>

<h2>3 – Neden <code>equals</code> ve <code>hashCode</code>? Ne Zaman Override Edilir?</h2>
<p>
Java’da iki nesnenin bellek adresi yerine, “içerik” veya “kimlik” bazında eşit olup olmadığını kontrol etmek için <code>equals</code> metodunu override ederiz. E-ticarette sıklıkla <strong>ürün</strong> nesnelerinin <code>productId</code> alanına göre eşitlik kontrolü yapılır. Örneğin iki farklı <code>Product</code> nesnesi, eğer aynı <code>productId</code> değerine sahipse “aynı ürün” olarak kabul edilir.
</p>

<pre><code>public class Product {
    private String productId;
    private String name;

    // Constructor vs.

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Product)) return false;
        Product other = (Product) obj;
        return this.productId.equals(other.productId);
    }

    @Override
    public int hashCode() {
        return productId.hashCode();
    }
}
</code></pre>

<p>
<code>hashCode</code> ise <code>HashMap</code> veya <code>HashSet</code> gibi hash tabanlı koleksiyonlarda verimli arama ve ekleme için kullanılır. Eğer <code>equals</code> metodunu değiştirirsek, aynı zamanda <code>hashCode</code> metodunu da tutarlı olacak şekilde override etmemiz gerekir.
</p>

<h2>4 – Java’da Diamond Problemi ve Nasıl Çözülür?</h2>
<p>
Diamond problemi, bir sınıfın, farklı ata sınıflarda tanımlanmış aynı metodu (veya özelliği) miras aldığı zaman ortaya çıkan belirsizliktir. C++ gibi dillerde birden fazla sınıfı doğrudan kalıtım yoluyla genişletmek (<em>multiple inheritance</em>) bu sorunu yaratır. Java’da ise sınıflarda çoklu kalıtım desteklenmediği için diamond problemi büyük ölçüde ortadan kalkar.
<br><br>
Ancak interface’lerin <code>default</code> metotları ile benzer bir çakışma oluşabilir. İki interface aynı <code>default</code> metodu sağlıyorsa, o metodu implement eden sınıf içerisinde açıkça override etmek gerekir:
</p>

<pre><code>interface Shippable {
    default void ship() {
        System.out.println("Shippable interface’inden gönderim yapılıyor.");
    }
}

interface Trackable {
    default void ship() {
        System.out.println("Trackable interface’inden gönderim yapılıyor.");
    }
}

class Order implements Shippable, Trackable {
    @Override
    public void ship() {
        // Hangi interface'in ship metodu çalışacak?
        Shippable.super.ship();
        Trackable.super.ship();
    }
}
</code></pre>

<p>
Bu şekilde hangisinin kullanılacağı belirtilebilir.
</p>

<h2>5 – Garbage Collector Neden Gerekli? Nasıl Çalışır?</h2>
<p>
Java’da bellek yönetimini otomatik olarak gerçekleştiren <strong>Garbage Collector (GC)</strong>, artık referans verilmeyen nesneleri tespit ederek bellekten temizler. E-ticaret sitelerinde sürekli yeni <code>Product</code>, <code>Order</code> nesneleri oluşturup sonradan bunlara ihtiyaç duymayabiliriz. GC bu gereksiz nesneleri temizleyerek bellek tasarrufu sağlar.
</p>
<ul>
    <li><strong>Nasıl Çalışır?</strong> JVM periyodik olarak heap üzerinde “ulaşılamayan” nesneleri işaretler ve temizler.</li>
    <li><strong>Ne Zaman Çalışır?</strong> Tam olarak zamanı garanti edilmemekle birlikte, bellek kullanımı belirli bir eşiğe ulaşınca veya JVM uygun gördüğünde devreye girer.</li>
    <li><code>System.gc()</code> ile “GC isteği” yapılsa bile derhal çalışacağının garantisi yoktur. JVM’in inisiyatifindedir.</li>
</ul>

<h2>6 – <code>static</code> Anahtar Kelimesi Kullanımı</h2>
<p>
<code>static</code>, bir üyeyi <em>sınıf</em> seviyesine taşır, örnek oluşturmaya gerek kalmadan erişilebilir kılar. E-ticaret projelerinde örneğin, fiyat hesaplama gibi genel araç metotlar için “utility” sınıflarda <code>static</code> tanımlanabilir:
</p>

<pre><code>public class PriceUtils {
    public static double applyDiscount(double price, double discountRate) {
        return price * (1 - discountRate);
    }
}
</code></pre>

<ul>
    <li><code>static</code> değişkenler tüm örneklerce ortak kullanılır.</li>
    <li><code>static</code> bloklar ise sınıf yüklendiğinde bir defaya mahsus çalışır.</li>
    <li><code>static</code> metotlar üzerinden, sınıfın instance üyelerine doğrudan erişilemez.</li>
</ul>

<h2>7 – Immutability Nedir? Nerede, Nasıl ve Neden Kullanılır?</h2>
<p>
Bir nesnenin <strong>immutability</strong> özelliği, oluşturulduktan sonra iç durumunun değiştirilememesi anlamına gelir. Mesela e-ticarette “para birimi ve tutar” gibi bir değerin yanlışlıkla değiştirilmesini istenmez. Immutable yapılırsa, “fiyat” bir kez belirlendiğinde daha sonradan farklı yerlerde manipüle edilemez.
</p>
<ul>
    <li><strong>Nerede Kullanılır?</strong> Sıkça <code>String</code>, <code>BigDecimal</code> gibi Java sınıflarında görülür. Finansal işlemlerde ve verinin paylaşıldığı çok katmanlı sistemlerde faydalıdır.</li>
    <li><strong>Nasıl Yapılır?</strong> Tüm alanları <code>private final</code> tanımlanır, setter metot verilmez, varsa koleksiyon alanları savunma kopyasıyla saklanır.</li>
    <li><strong>Neden?</strong> <em>thread-safe</em> olurlar, paylaşımda beklenmedik değişikliklerin önüne geçilir. Ayrıca kodu anlamak ve test etmek daha kolaydır.</li>
</ul>

<h2>8 – Composition ve Aggregation Nedir? Farkları Nelerdir?</h2>
<ul>
    <li><strong>Composition:</strong> Bir nesne diğerinin “yaşam döngüsüne” sıkı sıkıya bağlıdır. Örneğin, “Sipariş” içindeki “Sipariş Kalemleri (OrderItem)” nesneleri olmadan siparişin tam bir anlamı yoktur. Sipariş silinirse, içindeki Sipariş Kalemleri de silinir.</li>
    <li><strong>Aggregation:</strong> Daha gevşek bir bağlılık vardır. Örneğin, “Müşteri” ile “Adres” ilişkisi. Müşteri silinse bile adres kayıtları başka yerlerde kullanılabiliyor olabilir ya da adresler sistemde ayrı tutulabilir.</li>
</ul>
<p><strong>Composition</strong> = güçlü sahiplik, <strong>Aggregation</strong> = zayıf sahiplik.</p>

<h2>9 – Cohesion ve Coupling Nedir? Farkları Nelerdir?</h2>
<ul>
    <li><strong>Cohesion (Uyumluluk):</strong> Bir sınıfın veya modülün içindeki sorumlulukların birbirine ne kadar odaklı olduğu ile alakalı bir kavramdır. Örneğin <code>InventoryService</code> sadece stok yönetimi ile ilgili metotlar içeriyorsa “yüksek cohesion” vardır.</li>
    <li><strong>Coupling (Bağlılık):</strong> Sınıfların veya modüllerin birbirine ne ölçüde bağımlı olduğu ile alakalı bir kavramdır. <code>OrderService</code> sınıfının, <code>ProductService</code>’in ayrıntılarına çok fazla bağımlı olması yüksek coupling’e yol açar. Değiştirirken kırılma riski artar.</li>
</ul>
<p>
İyi mimarilerde hedef, <em>yüksek cohesion</em> ve <em>düşük coupling</em>tir. Bu sayede kod daha sürdürülebilir, anlaşılır ve genişletilebilir olur.
</p>

<h2>10 – Heap ve Stack Nedir? Farkları Nelerdir?</h2>
<ul>
    <li><strong>Stack:</strong> Metot çağrıları, yerel değişkenler ve referanslar tutulur. Fonksiyonun (metot) çalışması tamamlandığında buradaki alan otomatik silinir. Okuma-yazma çok hızlıdır ancak alanı sınırlıdır.</li>
    <li><strong>Heap:</strong> Nesnelerin bellekte tutulduğu geniş bir havuzdur. Java’nın Garbage Collector’ı ile yönetilir. <code>new</code> anahtar sözcüğü ile oluşturduğumuz nesneler genellikle heap’te yer alır. Erişimi stack’e göre daha yavaştır ancak çok daha büyük bir alan sunar.</li>
</ul>
<p>
Özellikle büyük e-ticaret projelerinde çok sayıda nesne (ürün, kullanıcı, sipariş) heap’te yer alır, <em>stack</em> ise metot çağrısı anında kısa ömürlü bilgileri saklar.
</p>

<h2>11 – Exception Nedir? Tipleri Nelerdir?</h2>
<p>
Exception, çalışma sırasında oluşan beklenmedik hatalardır.
</p>
<ul>
    <li><strong>Checked Exception:</strong> Derleyici tarafından kontrol edilir, <code>try-catch</code> veya <code>throws</code> ile yönetmek gerekir <code>IOException</code>, <code>SQLException</code> gibi hatalar, e-ticaret sisteminizde veritabanına bağlanırken ortaya çıkabilecek hatalardır.</li>
    <li><strong>Unchecked (Runtime) Exception:</strong> Kodumuz çalışırken ortaya çıkar, derleyici bu hataları yakalayamaz (<code>NullPointerException</code>, <code>IndexOutOfBoundsException</code> vb.). Örneğin, <code>Cart</code> listesinde olmayan bir ürüne erişmeye çalışmak.</li>
    <li><strong>Error:</strong> JVM’in kritik hataları (<code>OutOfMemoryError</code> gibi). Uygulama düzeyinde yakalayıp devam etmek genelde mümkün değildir.</li>
</ul>
<p>
E-ticarette kullanıcı hatalarını, ödemeyle ilgili hataları veya veritabanı sorunlarını (örneğin stok tablosuna erişim) uygun şekilde yönetmek için exception'ları kullanmak önemlidir.
</p>

<h2>12 – ‘Clean Code’u En Kısa Nasıl Özetleriz?</h2>
<p>
“Clean code”, okuyan kişinin niyetinizi hızla anlayabileceği, bakımı ve genişletmesi kolay, gereksiz karmaşıklıklardan arındırılmış koddur. İyi adlandırmalar, tek sorumluluk prensibi, gereksiz tekrarları önleme (<em>SOLID, DRY, KISS, YAGNI</em>) ve açık, net yapılandırma önemlidir. Böylece hem ekibe yeni katılanlar hem de kodu sonradan inceleyecek olanlar için zahmetsizce (technical debt) geliştirilebilir ve sürdürülebilir bir proje ortaya çıkar.
</p>

<h2>13 – Java’da Method Hiding Nedir?</h2>
<p>
Eğer bir alt sınıfta, üst sınıftaki <strong>static</strong> metodun aynısını (aynı imza ile) tekrar tanımlanırsa, bu işlem “method hiding” (metot gizleme) olarak adlandırılır. Yani bu bir “override” değildir çünkü <strong>static</strong> metotlar sınıfa aittir. Derleme zamanı (compile time) hangi metot çağrılacağına karar verilir.
</p>

<pre><code>class Campaign {
    public static void info() {
        System.out.println("Kampanya bilgisi: Üst sınıf statik metot.");
    }
}

class SpecialCampaign extends Campaign {
    public static void info() {
        System.out.println("Özel kampanya bilgisi: Alt sınıf statik metot.");
    }
}

public class Test {
    public static void main(String[] args) {
        Campaign campaign = new SpecialCampaign();
        campaign.info(); // "Kampanya bilgisi: Üst sınıf statik metot." (method hiding)
        
        SpecialCampaign.info(); // "Özel kampanya bilgisi: Alt sınıf statik metot."
    }
}
</code></pre>

<p>
Statik metotlar nesneye göre değil, değişkenin sınıf tipine göre çalışır.
</p>

<h2>14 – Abstraction ve Polymorphism Arasındaki Fark Nedir?</h2>
<ul>
    <li><strong>Abstraction (Soyutlama):</strong> Bir sınıfın “ne yapacağını” tanımlar, “nasıl yapacağını” gizler. Örneğin bir <code>Payment</code> interface’inde <code>pay()</code> metodu tanımlandığında, nasıl ödeme alınacağı alt sınıfların (kredi kartı, PayPal, kapıda ödeme vb.) sorumluluğundadır.</li>
    <li><strong>Polymorphism (Çok Biçimlilik):</strong> Aynı arayüz veya metodun farklı alt sınıflarda farklı davranışlar göstermesidir. Mesela <code>pay()</code> metodu, <code>CreditCardPayment</code> için ayrı, <code>PayPalPayment</code> için ayrı çalışır. Kod “genel” bir referansla (<code>Payment payment;</code>) yazılsa bile, gerçek nesneye göre farklı metot gövdeleri çalışır (runtime polymorphism).</li>
</ul>
<p>
Bu ikili birlikte kullanıldığında (<em>abstract/interface + override</em>) e-ticaret sistemlerinde farklı ödeme yöntemlerini veya farklı kargo tiplerini tek bir çatı altında toplayabilir, yeni özellikler eklemeyi kolaylaştırır.
</p>