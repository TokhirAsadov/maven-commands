# Java `@SuppressWarnings` Warnings List

`@SuppressWarnings` annotatsiyasi Java'da kompilyator tomonidan chiqariladigan ogohlantirishlarni o'chirish uchun ishlatiladi. Quyida Java'da mavjud bo'lgan barcha standart ogohlantirish turlari va ularning tavsifi keltirilgan.

## Warnings List

| warning name | description |
| --- | --- |
| `all` | Barcha ogohlantirishlarni o'chiradi. Odatda maxsus holatlarda ishlatiladi, chunki u barcha ogohlantirishlarni yashiradi. |
| `auxiliaryclass` | class ichida yordamchi (auxiliary) class ishlatilganda chiqadigan ogohlantirishlarni o'chiradi. |
| `boxing` | Primitiv turlar va ularning wrapper classlari o'rtasidagi avtomatik konversiya (boxing/unboxing) haqidagi ogohlantirishlarni o'chiradi. |
| `cast` | Xavfsiz bo'lmagan tur konversiyalari haqidagi ogohlantirishlarni o'chiradi. |
| `classfile` | class fayllarining tuzilishi yoki formatidagi muammolar haqidagi ogohlantirishlarni o'chiradi. |
| `deprecation` | Eskirgan (deprecated) API'lar ishlatilganda chiqadigan ogohlantirishlarni o'chiradi. |
| `dep-ann` | Eskirgan annotatsiyalar ishlatilganda chiqadigan ogohlantirishlarni o'chiradi. |
| `exports` | Modul tizimida noto'g'ri eksport qilingan paketlar haqidagi ogohlantirishlarni o'chiradi. |
| `fallthrough` | `switch` iborasida `break` ishlatilmaganda chiqadigan ogohlantirishlarni o'chiradi. |
| `finally` | `finally` blokida resurslar yoki xatti-harakatlar bilan bog'liq muammolar haqidagi ogohlantirishlarni o'chiradi. |
| `hiding` | O'zgaruvchilar yoki metodlarning ko'rinishini yashirish (shadowing) haqidagi ogohlantirishlarni o'chiradi. |
| `incomplete-switch` | `switch` iborasida enum yoki boshqa turlar uchun to'liq qamrov bo'lmaganda chiqadigan ogohlantirishlarni o'chiradi. |
| `lossy-conversions` | Ma'lumot yo'qotilishi mumkin bo'lgan tur konversiyalari (masalan, `double` dan `int` ga) haqidagi ogohlantirishlarni o'chiradi. |
| `missing-explicit-ctor` | Konstruktor aniq belgilab o'tilmagan classlar haqidagi ogohlantirishlarni o'chiradi. |
| `module` | Modul tizimida noto'g'ri konfiguratsiya yoki foydalanish haqidagi ogohlantirishlarni o'chiradi. |
| `nls` | Non-Localized String (xalqaro tilga moslashtirilmagan matnlar) haqidagi ogohlantirishlarni o'chiradi. |
| `null` | Null-pointer xavfi haqidagi ogohlantirishlarni o'chiradi. |
| `opens` | Modul tizimida noto'g'ri ochilgan paketlar haqidagi ogohlantirishlarni o'chiradi. |
| `overloads` | Metodlarning noto'g'ri overload qilinishi haqidagi ogohlantirishlarni o'chiradi. |
| `overrides` | Metodlarning noto'g'ri override qilinishi haqidagi ogohlantirishlarni o'chiradi. |
| `preview` | Preview xususiyatlar (Java'ning eksperimental xususiyatlari) ishlatilganda chiqadigan ogohlantirishlarni o'chiradi. |
| `processing` | Annotatsiya protsessorlari bilan bog'liq muammolar haqidagi ogohlantirishlarni o'chiradi. |
| `rawtypes` | Generik turlardan foydalanmaslik haqidagi ogohlantirishlarni o'chiradi. |
| `removal` | Kelajakda olib tashlanishi rejalashtirilgan API'lar ishlatilganda chiqadigan ogohlantirishlarni o'chiradi. |
| `requires-automatic` | Modul tizimida avtomatik modullarga bog'liqlik haqidagi ogohlantirishlarni o'chiradi. |
| `requires-transitive-automatic` | Modul tizimida transitive avtomatik modullarga bog'liqlik haqidagi ogohlantirishlarni o'chiradi. |
| `resource` | Resurslar (masalan, `try-with-resources` da yopilmagan resurslar) haqidagi ogohlantirishlarni o'chiradi. |
| `serial` | `Serializable` interfeysini amalga oshiruvchi classlarda `serialVersionUID` maydonining yo'qligi haqidagi ogohlantirishni o'chiradi. |
| `static` | Statik metodlar yoki maydonlarga noto'g'ri murojaat qilish haqidagi ogohlantirishlarni o'chiradi. |
| `strictfp` | `strictfp` kalit so'zi bilan bog'liq muammolar haqidagi ogohlantirishlarni o'chiradi. |
| `synchronization` | Sinxronizatsiya bilan bog'liq muammolar (masalan, noto'g'ri `synchronized` bloklar) haqidagi ogohlantirishlarni o'chiradi. |
| `synthetic` | Kompilyator tomonidan yaratilgan sintetik konstruktsiyalar haqidagi ogohlantirishlarni o'chiradi. |
| `try` | `try-with-resources` bloklaridagi muammolar haqidagi ogohlantirishlarni o'chiradi. |
| `unchecked` | Xavfli turdagi konversiyalar (masalan, generiklardan foydalanishdagi xatolar) haqidagi ogohlantirishlarni o'chiradi. |
| `unnecessary` | Keraksiz kod yoki annotatsiyalar (masalan, foydalanilmagan `@SuppressWarnings`) haqidagi ogohlantirishlarni o'chiradi. |
| `unused` | Ishlatilmagan o'zgaruvchilar, metodlar yoki classlar haqidagi ogohlantirishlarni o'chiradi. |
| `varargs` | O'zgaruvchan argumentlar (`varargs`) bilan bog'liq xavfli foydalanish haqidagi ogohlantirishlarni o'chiradi. |

## Qo'shimcha Eslatmalar

- `@SuppressWarnings` annotatsiyasini imkon qadar aniq ogohlantirishlar uchun ishlatish tavsiya etiladi (masalan, `"unchecked"` yoki `"deprecation"`). `"all"` dan foydalanish kod sifatini pasaytirishi mumkin.
- Annotatsiyani eng kichik doirada (masalan, metod yoki o'zgaruvchi darajasida) qo'llash yaxshidir, class darajasida ishlatish ogohlantirishlarni haddan tashqari yashirishi mumkin.
- Har bir ogohlantirishni o'chirishdan oldin, uni tuzatish imkoniyati bor-yo'qligini ko'rib chiqing, chunki ogohlantirishlar potentsial xatolarni aniqlashga yordam beradi.
- Ba'zi ogohlantirishlar (masalan, `preview` yoki `module`) Java versiyasiga bog'liq bo'lib, faqat yangi versiyalarda ishlaydi.
