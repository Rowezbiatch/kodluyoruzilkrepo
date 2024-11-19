# Css kalp yapımı

* Kare bir eleman kullanarak tanımlıyoruz
```style.css
aspect-ratio: 1
```
Son görüntüsü ↓
![Örnek](https://verpex.com/assets/uploads/images/blog/heart-shape.png?v=1683297918)
* Ardından, son şekli oluşturmak için üç gradient katmanı kullanırız. Her  gradient anlamak için bir örnek
![Örnek](https://verpex.com/assets/uploads/images/blog/overview-gradients.png?v=1683298241)
* Her `radial-gradient()` şeklin dairesel kısmını oluşturmak için üstteki elemanın dörtte birini alacaktır `conic-gradient()` altta üçgen parça oluşturacaktır.

* Her zaman aynı yapılandırmadır:  `radial-gradient()` ve bir `conic-gradient()` farklı yazılmalı.
* Bazen, kenar yumuşatma ile ilgili bazı sorunlarla karşılaşabiliriz. `conic-gradient()` ve pürüzlü kenarlar alıyoruz. Bundan kaçınmak için `clip-path:`
 ```
 .heart {
  --c: red;
  width: 200px;
  aspect-ratio: 1;
  background:
   radial-gradient(at 70% 31%,var(--c) 29%,#0000 30%),
   radial-gradient(at 30% 31%,var(--c) 29%,#0000 30%),
   linear-gradient(var(--c) 0 0) bottom/100% 50% no-repeat;
  clip-path: polygon(-43% 0,50% 91%, 143% 0);
}
```
* Ben değiştirdim `conic-gradient()` ile `linear-gradient()` alt alanı ana renkle kaplamak için. Sonra `clip-path ` öğeyi klipsleyecek ve üçgen şekli (triangular shape) oluşturacaktır:


![Burdaki gibi](https://verpex.com/assets/uploads/images/blog/overview-clip-path.png?v=1683298462)
## Border İmage kullanma

* Kodumuzu optimize edebilir ve üç gradients yalnızca bir tanesiyle değiştirebiliriz. Evet, mümkün! Bunun için güveneceğiz `border-image`ve  `radial-gradient()`kullanarak.

```
.heart {
  --c: red;
  width: 200px;
  aspect-ratio: 1;
  border-image: radial-gradient(var(--c) 69%,#0000 70%) 84.5%/50%;
  clip-path: polygon(-42% 0,50% 91%, 142% 0);
}
```
* Biliyorum ki `border-image` sözdizimi biraz çılgınca ama inceleyelim. Bunun kısaltması:
```
border-image-source: radial-gradient(var(--c) 69%,#0000 70%);
border-image-slice: 84.5%;
border-image-width: 50%;

```

* `` radial-gradient() ``'a ihtiyacımız var dairesel bir şekil oluşturmak için border``-image-source`` önemsiz.
 
* Genişliğin mümkün olduğunca büyük olması için tüm elemti kaplayacak şekle ihtiyacımız var. 50% minimum değerdir ancak daha büyük yapabiliriz ve sonuç her zaman aynı olacaktır. Daha az 50% bize bir boşluk bırakacak. Örneğin, 45% eşit bir boşluğunuz olacak 100% - 2*45% = 5% ortada.

En zor değer ``border-image-slice`` Kullandığınızda 100% dilim içinde, her köşede bir daire olacaktır. 50% kullanmak size tüm öğeyi kapsayan tam bir daire verecektir. Arasındaki değerle manuel olarak oynarsanız 50% ve 100% ihtiyacımız olan şekli alabilirsiniz. ``Değer 84.5%``
* Son olarak, clip-path uyguluyoruz ve kalp şeklini alıyoruz:
 
![Burdaki gibi](https://verpex.com/assets/uploads/images/blog/border-image-clip-path.png?v=1683298698)

## İmage'nizi kalbin içine koyma
* Bir kalp şekli güzel ancak içine resim koymak daha iyidir! Bu amaçla, önceki kodu yeniden kullanacağız ve `mask` yöntemini kullanacağız.ve `background` kullanıcaz . `mask` kullanılcak ve `mask-border` 

`* Neden sadece en kısa olan son sürümü kullanmıyorsunuz?`

Gelecek için tavsiye ederim ama şimdilik, `mask-border` çok düşük bir tarayıcı desteğine sahiptir, bu nedenle bir geri dönüş olarak `mask` özelliğine güvenir.

```
img {
  width: 250px;
  aspect-ratio: 1;
  --_m: radial-gradient(#000 69%,#0000 70%) 84.5%/50%;
  -webkit-mask-box-image: var(--_m);
             mask-border: var(--_m);
  clip-path: polygon(-42% 0,50% 91%, 142% 0);
}

/* fallback until better support for mask-border */
@supports not (-webkit-mask-box-image: var(--_m)) { 
  img {
   --_m:
     radial-gradient(at 70% 31%,var(--c) 29%,#0000 30%),
     radial-gradient(at 30% 31%,var(--c) 29%,#0000 30%),
     linear-gradient(#000 0 0) bottom/100% 50% no-repeat;
   -webkit-mask: var(--_m);
           mask: var(--_m);
  }
}
```
TADA! İmajımızı küçük bir kodla kalp şekline dönüştürdük. Ön ekler ve tarayıcı desteği nedeniyle kod biraz uzun görünüyor, ancak ileri zamanlarda sadece bu olacak:

```
img {
  width: 250px;
  aspect-ratio: 1;
  mask-border: radial-gradient(#000 69%,#0000 70%) 84.5%/50%
  clip-path: polygon(-42% 0,50% 91%, 142% 0);
}
```
## Sonuç
Girişte söylediğim gibi: pseudo-element yok border-radius yok ve  rotation yok. Bir eleman ve birkaç kod satırı kullanarak kalp şekli oluşturmak için modern CSS hilelerini kullandık. Bu yazıyı bir CSS deseni. Kalbi oluşturmak için gradients kullandık böylece onunla kolayca bir kalp oluşturabiliriz!


[Kaynakca](https://verpex.com/blog/website-tips/css-shapes-the-heart)