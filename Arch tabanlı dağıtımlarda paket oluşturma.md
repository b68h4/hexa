# Arch linux package distribution

Arch linux ve tabanlı dağıtımlarda **paket inşa etmek için** `makepkg` komutunu kullanırız. Bu rehberimde basitçe "Merhaba Dünya!" yazdıran paket yapacağız. Bu komutları PKGBUILD dosyasının içine  
gireceksiniz.

---

Temel Bilgiler

PKGBUILD dosyaları, tümü paketin kendisini ve nasıl oluşturulacağını tanımlamak için kullanılan değişkenlerden ve işlevlerden oluşur.

---

PKGBUILD Dosyası Oluşturma

-   `pkgname`: Bu, kurulum sırasında paketinizin adını tanımlayan ve Arch Linux'un paket yöneticisi pacman'ın paketi nasıl takip ettiğini belirleyen şeydir. `pkgname="merhaba-dunya"`
-   `pkgver`: Paketin versiyonunu belirtir. `pkgver="1.0.0"`
-   `pkgdesc`: Paketin açıklamasının yazılacağı kısım. `pkgdesc="Merhaba Dünya!"`
-   `arch`: Paketin mimarisinin belirleneceği kısım. `arch=("x86_64")` Eğer birden fazla mimari belirlemek istiyorsanız şöyle yapmalısınız; `arch=(“x86_x64” “arm”)`
-   `depends`: Bu, paketimizin çalışması için ihtiyaç duyduğu tüm paketleri listeler. Arch gibi, birden çok değer içerebilir ve bu nedenle parantez sözdizimini kullanmalıdır.
    > Paketimizin herhangi bir bağımlılığı olmayacağı için PKGBUILD'de bu alana girmemize gerek yok. Ancak paketimizin bağımlılıkları olsaydı, sadece arch ile aynı sözdizimini kullanırdık.
-   `optdepends`: Bu, çalışması gerekmeyen ancak ekstra işlevsellik için gerekli olan paketleri listeler.
-   `conflicts`: Bu, pacman'a hangi paketlerin paketimizin istemediğimiz bir şekilde hareket etmesine veya davranmasına neden olacağını söyler.
    Burada listelenen herhangi bir paket, bizimki kurulmadan kaldırılacaktır.
    Bu, bağlı olarak da aynı sözdizimini izler.
-   `license`: [Lisanslar hakkında bilgi edinmek için.](https://wiki.archlinux.org/title/PKGBUILD#license)
-   `source`: Makepkg, paketimizi oluşturmak için hangi dosyaları kullanacağını bu şekilde bilir. Bu, yerel dosyalar ve URL'ler dahil olmak üzere çeşitli farklı türde kaynaklar içerebilir.
    Yerel dosyalar eklerken, dosyanın adını PKGBUILD'e göre girin, yani aşağıdaki dizin düzenini göz önünde bulundurun:
    -   PKGBUILD
    -   dosya.txt
    -   src/dosya.sh  
        `source=("merhaba-dunya.sh")`
-   `sha512sums`: Bu, kaynaktaki dosyaların değiştirilmediğini veya yanlış indirilmediğini doğrulamak için kullanılır. Bunun için değerlerin alınmasıyla ilgili bilgiler, PKGBUILD'ler hakkındaki Arch Wiki makalesinde bulunabilir.  
     Bunu ayarlamamayı tercih ediyorsanız (veya yerel dosyalar için yapmanız gerekmiyorsa), kaynak değişkendeki her dosya için SKIP girebilirsiniz: `sha512sums=("SKIP")`
-   `package():`

Bu, paketimizi gerçekten yapmanın son ve en önemli kısmıdır. Bununla çalışırken iki değişkeni bilmek önemlidir:

-   ${srcdir}: Burası makepkg'nin dosyaları kaynak değişkene koyduğu yerdir. Bu, dosyalarla etkileşime girebileceğiniz ve dosyalarda gerekli diğer değişiklikleri yapabileceğiniz dizindir
-   ${pkgdir}: Sistemimize kurulacak dosyaları buraya yerleştiriyoruz.
-   ${pkgdir} klasör yapısı sanki gerçek bir sistemdeymiş gibi kurulur (yani ${pkgdir}/usr/bin/merhaba-dunya pacman ile kurulum yaparken /usr/bin/merhaba-dunya dosyasını oluşturur.

```
package() { echo 'Merhaba Dünya!' > "${srcdir}/merhaba-dunya.sh" mkdir -p "${pkgdir}/usr/bin" cp "${srcdir}/merhaba-dunya.sh" "${pkgdir}/usr/bin/merhaba-dunya" chmod +x "${pkgdir}/usr/bin/merhaba-dunya" }
```

Ve hazırız eğer bittiyse şöyle gözükecek:
![[68747470733a2f2f666f72756d2e736869667464656c6574652e6e65742f656b6c65722f313632373338333136373731362d706e672e3136303132302f.png]]

> PKGBUILD ile yazdığınız kod aynı klasörde olmalıdır.

> Merhaba-dunya.sh oluşturmak için `touch merhaba-dunya.sh`

> Bu sadece örnekti, siz istediğiniz her türlü komutu paketin içine yerleştirebilirsiniz.
