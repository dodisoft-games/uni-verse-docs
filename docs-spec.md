# Birden fazla dökümantasyonu içerisinde barındırabilen ve dil desteği sunan json tabanlı dökümantasyon sistemi

Dosya yapısı:
- `docs` adındaki klasörün içerisinde her bir dökümantasyon için bir klasör bulunur ve klasör ismi dökümantasyon'un id değeri olmalıdır.
- Her bir dökümantasyon klasörünün içerisinde dil seçenekleri için klasörler bulunur. (`en-US`, `tr-TR`, ...)
- Her bir dil seçeneği içerisinde navigasyonu belirtmek için bir adet `nav.json` dosyası ve sayfaları belirtmek için `pages` adında bir klasör bulunur.
- `pages` klasörü içerisinde ise her dökümantasyon sayfası için bir adet json dosyası bulunur. json dosyalarının ismi ilgili sayfanın id'si olmalıdır. (`uniscript-info.json`)

Örnek bir sayfanın yolu: `docs/uniscript/en-US/pages/uniscript-info.json`

## Bir sayfanın json yapısı ve hiyerarşisi

```json
{
    "id": "",
    "lang": "",
    "name": "",
    "description": "",
    "link": "",
    "tags": [],
    "previous": {
        "name": "",
        "link": ""
    },
    "next": {
        "name": "",
        "link": ""
    },
    "content": [
        {
            "id": "",
            "tag": "",
            "styles": ,
            "classes": 
        },
        ...
    ]
}
```

| Alan | Açıklama |
|---|---|
| `id` | Sayfanın id'sini belirtir. (`usc-arrays`, `usc-variables`, ...) |
| `lang` | Sayfanın dilini belirtir. (`en-US`) |
| `name` | Sayfanın ismini belirtir. (`UniScript Arrays`, `UniScript Variables`, ...) |
| `description` | Sayfanın açıklamasını belirtir. |
| `link` | Sayfanın yayınlandığı linki belirtir. İlgili dosyanın yolu ile aynıdır. Ancak `pages` ve uzantı belirtilmez: (`/docs/uniscript/en-US/uniscript-info`) |
| `tags` | Sayfa içeriği ile ilgili tag'leri belirtir. `["uniscript", "intro", "overview", "web", "scripting"]` |
| `previous` | Mevcut sayfadan bir önceki sayfayı belirtir. Gerekirse kullanılır. |
| `next` | Mevcut sayfadan bir sonraki sayfayı belirtir. Gerekirse kullanılır. |
| `content` | Sayfa içeriğini belirtir. İçerikler sırayla block halinde render'lanır. |

`previous` ve `next` içerisindeki:

| Alan | Açıklama |
|---|---|
| `name` | İlgili sayfanın ismini belirtir. Bu değerler boş ise bir önceki/sonraki sayfa yok demektir. |
| `link` | İlgili sayfanın linkini belirtir. Bu değerler boş ise bir önceki/sonraki sayfa yok demektir. |

`content` içerisindeki verilerde bulunan:

| Alan | Açıklama |
|---|---|
| `id` | İçeriğin id'sini belirtir. (`block-001`, `block-002` vs.) |
| `tag` | İçeriğin türünü belirtir. İçerik türüne göre bu değerlere ekstra değerler eklenir. |
| `styles` | İçeriğe özel olarak uygulanmak istenen css değerlerini belirtir. → `"styles": { "margin-bottom": "1rem", "color": "red" }` gibi. Bu değer `null` olabilir. |
| `classes` | İçeriğe özel olarak verilmek istenen class değerlerini belirtir. → `"classes": [ "animation-1", "hoveredEffect" ]` gibi. Bu değer `null` olabilir. |

`tag` değerine göre ekstra keyler eklenir. Mevcut `tag` değerleri:

| Tag | Açıklama |
|---|---|
| `button` | Bir buton ekler. |
| `h1` | h1 başlığı ekler. |
| `h2` | h2 başlığı ekler. |
| `h3` | h3 başlığı ekler. |
| `h4` | h4 başlığı ekler. |
| `p` | Paragraf ekler. |
| `important` | Bilgilendirme için kart ekler. Kartın rengi mavidir. |
| `note` | Bilgilendirme için kart ekler. Kartın rengi mordur. |
| `remove` | Bilgilendirme için kart ekler. Kartın rengi kırmızıdır. |
| `tip` | Bilgilendirme için kart ekler. Kartın rengi yeşildir. |
| `warning` | Bilgilendirme için kart ekler. Kartın rengi turuncudur. |
| `code` | Kod alanı ekler. |
| `image` | Resim ekler. |
| `video` | Video ekler. |
| `iframe` | iframe ekler. |
| `ol` | Sıralı liste ekler. |
| `ul` | Sırasız liste ekler. |
| `table` | Tablo ekler. |

---

`tag` değeri eğer `button`, `h1`, `h2`, `h3`, `h4`, `important`, `note`, `p`, `remove`, `tip`, `warning` ise `text` alanı eklenir:

```json
{
    "id": "",
    "tag": "",
    "styles": {},
    "classes": [],
    "text": ""
}
```

---

`tag` değeri eğer `code` ise `lines` alanı eklenir (içerikteki kodları array halinde satır satır belirtir):

```json
{
    "id": "",
    "tag": "",
    "styles": {},
    "classes": [],
    "lines": [
        "using System;",
        "",
        "namespace Application",
        "{",
        "    class Program",
        "    {",
        "        static void Main(string[] args)",
        "        {",
        "            Console.WriteLine(\"Hello World!\");",
        "        }",
        "    }",
        "}"
    ]
}
```

---

`tag` değeri eğer `image`, `video`, `iframe` ise aşağıdaki alanlar eklenir:

| Alan | Açıklama |
|---|---|
| `type` | İçerikte kaç adet resim/video/yerleştirme olduğunu `number` türünden belirtir. min: 1, max: 6 |
| `src` | `type` değeri kadar url'yi içerisinde sırasına göre array halinde belirtir: `"src": [ "/img-1", "/img-2" ]` |
| `alt` | `type` değeri kadar alternative text'i sırasına göre array halinde belirtir: `"alt": [ "lorem ipsum", "dolor" ]` |

```json
{
    "id": "",
    "tag": "",
    "styles": {},
    "classes": [],
    "type": 2,
    "src": [
        "/img-1",
        "/img-2"
    ],
    "alt":[
        "lorem ipsum",
        "dolor"
    ]
}
```

---

`tag` değeri eğer `ol`, `ul` ise `list` alanı eklenir:

- `list`: İçerikteki listeleri array halinde tutar. Liste değeri `list` içerisindeki verilerdeki `t` değeridir.
- Array içerisindeki verilerde `t` değerinin yanında `c` değeri de olabilir. Eğer `c` değeri varsa bu seçeneğin alt seçenekleri de mevcuttur.
- `c` değeri verileri array halinde tutar. İçerikleri ise aynı şekilde `t` veya `t`, `c` şeklindedir. (`t`: text, `c`: children)

```json
{
    "id": "",
    "tag": "",
    "styles": {},
    "classes": [],
    "list": [
        {
            "t": "list-1"
        },
        {
            "t": "list-2",
            "c": [
                {
                    "t": "list-2.1"
                },
                {
                    "t": "list-2.2",
                    "c": [
                        {
                            "t": "list-2.2.1"
                        }
                    ]
                }
            ]
        },
        {
            "t": "list-3"
        }
    ]
}
```

---

`tag` değeri eğer `table` ise aşağıdaki alanlar eklenir:

| Alan | Açıklama |
|---|---|
| `headers` | Oluşturulacak tablonun başlığını array halinde belirtir. |
| `rows` | Oluşturulacak tablonun içeriğini array halinde belirtir. |

```json
{
    "id": "",
    "tag": "",
    "styles": {},
    "classes": [],
    "headers": ["First Name", "Last Name", "Age"],
    "rows": [
        ["James", "Smith", 28],
        ["Emily", "Johnson", 34],
        ["Michael", "Brown", 41]
    ]
}
```

---

### `tag` değerlerine göre eklenecek ekstra alanlar özeti

| Tag | Ekstra Alan(lar) |
|---|---|
| `button` | `text` |
| `h1` | `text` |
| `h2` | `text` |
| `h3` | `text` |
| `h4` | `text` |
| `p` | `text` |
| `important` | `text` |
| `note` | `text` |
| `remove` | `text` |
| `tip` | `text` |
| `warning` | `text` |
| `code` | `lines` |
| `image` | `type`, `src`, `alt` |
| `video` | `type`, `src`, `alt` |
| `iframe` | `type`, `src`, `alt` |
| `ol` | `list` (`t`, `c`) |
| `ul` | `list` (`t`, `c`) |
| `table` | `headers`, `rows` |

---

## `text`, `list` içerisindeki `t` ve `headers` / `rows` içerisindeki tutucularda kullanılan metin biçimlendiriciler

| Sözdizimi | Açıklama |
|---|---|
| `**bold**` | Metni kalın yazar |
| `*italic*` | Metni italik yazar |
| `__underline__` | Metnin altını çizer |
| `~~Strike-through~~` | Metnin üzerini çizer |
| `` `code` `` | Metni kod şeklinde yazar |
| `[bridge](/pages/bridge)` | Metne site içinde köprü kurar |
| `[block](/pages/bridge#block-001)` | Metne site içerisindeki bir bloğa yönlendirme yapar |
| `[link](https://www.google.com/)` | Metne dışarıya link verir |
| `\n` | Alt satıra geçer |

### Örnek json

```json
{
    "id": "uniscript-info",
    "lang": "en-US",
    "name": "UniScript Overview",
    "description": "A high-level introduction to UniScript, its history, and its role in modern web development.",
    "link": "/docs/uniscript/en-US/uniscript-info",
    "tags": ["UniScript", "intro", "overview", "web", "scripting"],
    "previous": {
        "name": "",
        "link": ""
    },
    "next": {
        "name": "Variables",
        "link": "/docs/uniscript/en-US/js-variables"
    },
    "content": [
        {
            "id": "block-001",
            "tag": "h1",
            "styles": null,
            "classes": null,
            "text": "uniscript Overview"
        },
        {
            "id": "block-002",
            "tag": "p",
            "styles": null,
            "classes": null,
            "text": "uniscript was created in __1995__ by *Brendan Eich* while working at `Netscape` Communications. Originally named *Mocha*, it was later renamed *LiveScript* and finally **uniscript**. Despite the similar name, uniscript is fundamentally different from Java — the naming was a [marketing decision](/pages/marketing)."
        },
        {
            "id": "block-003",
            "tag": "video",
            "styles": null,
            "classes": null,
            "type": 2,
            "src": [
                "/video-1",
                "/video-2"
            ],
            "alt":[
                "lorem ipsum",
                "dolor"
            ]
        },
        {
            "id": "block-004",
            "tag": "code",
            "styles": null,
            "classes": null,
            "lines": [
                "let score = 100;",
                "score = 200; // Reassignment is allowed",
                "",
                "if (true) {",
                "    let blockVar = \"only inside this block\";",
                "    console.log(blockVar); // \"only inside this block\"",
                "}",
                "",
                "// console.log(blockVar); // ReferenceError: blockVar is not defined"
            ]
        },
        {
            "id": "block-005",
            "tag": "ul",
            "styles": null,
            "classes": null,
            "list": [
                {"t": "list-1"},
                {"t": "list-2", "c": [{"t": "list-2.1"}]},
                {"t": "list-3"}
            ]
        },
        {
            "id": "block-006",
            "tag": "table",
            "styles": null,
            "classes": null,
            "headers": ["", "Windows", "macOS"],
            "rows": [
                ["**background**", "3.1", "1.2"],
                ["**svg**", "2.0.0.6", "1.2"],
                ["**clip**", "1.0", "~~invalid~~"]
            ]
        }
    ]
}
```

---

# nav.json dosyası ve hiyerarşisi

Oluşturulan her dosyanın hiyerarşisi `nav.json` dosyasında tutulur.

```json
{
    "items": [
        {
            "id": "",
            "name": "",
            "link": "",
            "bold": boolean,
            "inner": []
        }
    ]
}
```

`items`: Sayfa başlıklarını array halinde sıra sıra belirtir.

### `items` içerisindeki alanlar

| Alan | Açıklama |
|---|---|
| `id` | İlgili sayfanın id'sini belirtir. |
| `name` | İlgili sayfanın ismini belirtir. |
| `link` | İlgili sayfanın linkini belirtir. |
| `bold` | İlgili sayfanın isminin bold olup olmamasını belirtir. (`true`/`false`) |
| `inner` | İlgili sayfanın eğer mevcutsa alt kategorisini array halinde sıra sıra belirtir. |

- `id`, `name` ve `link` değerleri ilgili sayfa içerisindeki değerler ile aynı olmalıdır.
- `link` değeri boş veya `null` olabilir. Bu değer eğer tanımlanmamışsa kategori başlığı olarak varsayılır.
- `inner` içerisindeki değerler tıpkı üst değerler gibi aynı değerleri taşır. Bu sayede teorik olarak bir kategorinin sınırsız alt kategorisi olabilir. (Alt kategori yoksa bu değer `null` olur.)

```json
{
    "items": [
        {
            "id": "uniscript-info",
            "name": "UniScript Overview",
            "link": "/docs/uniscript/en-US/uniscript-info",
            "bold": true,
            "inner": null
        },
        {
            "id": "",
            "name": "",
            "link": null,
            "bold": true,
            "inner": [
                {
                    "id": "",
                    "name": "",
                    "link": "",
                    "bold": false,
                    "inner": [
                        {
                            "id": "",
                            "name": "",
                            "link": "",
                            "bold": false,
                            "inner": [
                                {
                                    "id": "",
                                    "name": "",
                                    "link": "",
                                    "bold": false,
                                    "inner": null
                                }
                            ]
                        }
                    ]
                },
                {
                    "id": "",
                    "name": "",
                    "link": "",
                    "bold": false,
                    "inner": null
                }
            ]
        }
    ]
}
```
