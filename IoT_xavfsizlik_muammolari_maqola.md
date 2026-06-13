# IoT TIZIMLARIDAGI XAVFSIZLIK MUAMMOLARI TAHLILI

**Muallif:** [Muallif ismi]
**Yo'nalish:** Axborot xavfsizligi / Kibertahlf
**Kalit so'zlar:** IoT xavfsizligi, kiber tahdidlar, kriptografiya, autentifikatsiya, DDoS, zaifliklar
**Sana:** 2026

---

## ANNOTATSIYA

Narsalar interneti (IoT — Internet of Things) tizimlarining jadal rivojlanishi va keng tarqalishi zamonaviy texnologiya dunyosining eng muhim hodisalaridan biri hisoblanadi. Biroq ushbu rivojlanish bilan birga kritik xavfsizlik muammolari ham yuzaga kelmoqda. Ushbu maqolada IoT tizimlarida mavjud asosiy xavfsizlik muammolari tizimli ravishda tahlil qilinadi: resurs cheklanishi, autentifikatsiya zaif tomonlari, shifrlash muammolari, DDoS hujumlari, firmware yangilash xavfsizligi va maxfiylik muammolari. Tahlil qilish jarayonida IEEE Xplore, SpringerLink, NIST va ENISA kabi obro'li manbalar qo'llanilgan. Maqolaning oxirida mavjud yechimlar va kelajakdagi tadqiqot yo'nalishlari tavsiya etiladi.

---

## 1. KIRISH

Narsalar interneti (IoT) texnologiyasi so'nggi yillarda kundalik hayotning barcha sohalariga kirib keldi — aqlli uylardan tortib sanoat avtomatlashtirishgacha, tibbiyotdan transport tizimlarigacha. Statista ma'lumotlariga ko'ra, 2025 yilda dunyo bo'yicha IoT qurilmalarining soni taxminan **19,8 milliard**ga yetgan bo'lib, bu ko'rsatkich 2034 yilga borib **40,6 milliarddan** oshishi kutilmoqda [Statista, 2025]. Global IoT bozorining qiymati esa 2025 yilda 419,8 milliard AQSh dollariga baholangan [Statista, 2025].

Ammo ushbu o'sish bilan birga jiddiy kibertahdidlar ham kuchaymoqda. IoT qurilmalari o'ziga xos cheklovlar — past hisoblash quvvati, cheklangan xotira, past energiya sarfi talabi va uzoq ishlash muddati — sababli an'anaviy axborot xavfsizligi mexanizmlari ularga to'g'ridan-to'g'ri qo'llanilishi qiyin. Bu holat IoT tizimlarini kiberhujumlar uchun qulay nishonga aylantiradi.

2016 yilgi **Mirai botnet** hujumi bu muammoning ko'lamini yaqqol namoyon qildi: yuzbinlab IoT qurilmalaridan foydalanib, internet infratuzilmasiga **1,1 Tbit/s** tezlikdagi DDoS hujumi uyushtirildi va bir qator yirik xizmatlarni vaqtincha ishdan chiqarildi [CISA, 2016; Cloudflare, 2017]. Ushbu voqea IoT xavfsizligini nafaqat akademik, balki davlat va sanoat darajasida ham ustuvor masalaga aylantirdi.

Ushbu maqolaning maqsadi — IoT tizimlaridagi xavfsizlik muammolarini qatlamlar bo'yicha tizimli tahlil qilish, mavjud yechimlarni ko'rib chiqish va kelajakdagi tadqiqot yo'nalishlarini belgilashdan iborat.

---

## 2. IoT ARXITEKTURASI VA XAVFSIZLIK QATLAMLARI

IoT tizimi odatda uch asosiy qatlamdan iborat [Springer, 2025; IEEE, 2016]:

| Qatlam | Tarkib | Asosiy tahdidlar |
|--------|--------|-----------------|
| **Perception Layer** (Idrok qatlami) | Sensorlar, aktuatorlar, RFID, kameralar | Fizik hujumlar, side-channel attacks, klonlash |
| **Network Layer** (Tarmoq qatlami) | Wi-Fi, Zigbee, BLE, MQTT, CoAP, 5G | Eavesdropping, Man-in-the-Middle, DDoS |
| **Application Layer** (Ilova qatlami) | Bulut platformalar, API, foydalanuvchi interfeysi | Autentifikatsiya zaif tomonlari, SQL injection, privilege escalation |

Ba'zi tadqiqotchilar ushbu modeli beshta qatlamga kengaytiradilar — Edge qatlami va Middleware qatlamini alohida ajratish orqali [ResearchGate, 2024]. Har bir qatlam o'z xavfsizlik talablarini qo'yadi va ular o'rtasida integratsiyalashgan himoya tizimi qurilishi zarur.

---

## 3. IoT TIZIMLARIDAGI ASOSIY XAVFSIZLIK MUAMMOLARI

### 3.1 Resurs Cheklanishi va Kriptografik Yechimlarning Qo'llanilmasligi

IoT qurilmalarining ko'pchiligi — ayniqsa, arzon sensorlar, RFID teglar va mikrokontrollerlar — juda cheklangan resursga ega:
- **RAM:** 2–256 KB
- **Flash xotira:** 32 KB–1 MB
- **Protsessor:** 8–32 bit, 1–200 MHz
- **Energiya:** batareyadan ishlash, yillab almashtirmaslik

Bu holat AES-256, RSA kabi standart kriptografik algoritmlarni amalda qo'llashni imkonsiz qiladi. Masalan, 8-bit MCU qurilmada AES-128 ni amalga oshirish hisoblash resurslarining 40% gacha sarflashini talab qilishi mumkin [NIST, 2023].

Ushbu muammoni hal etish uchun **Lightweight Cryptography** (engil kriptografiya) sohasi rivojlandi. NIST 2023 yil fevralida **ASCON** algoritmini engil kriptografiya standarti sifatida tanladi [NIST, 2023], 2025 yil avgustda esa rasmiy standart **NIST SP 800-232** sifatida nashr etildi [NIST SP 800-232, 2025]. ASCON quyidagi afzalliklarga ega:
- Permutatsiyaga asoslangan yengil tuzilma
- Authenticated Encryption (AEAD) va Hash funksiyalarini qo'llab-quvvatlash
- Resurs-cheklangan muhitlarda yuqori samaradorlik
- IoT, RFID, tibbiy implantlar uchun moslashtirilganlik

**Muammo holati:** Ko'plab mavjud IoT qurilmalari hali ham shifrlanmagan yoki zaif shifrlangan kommunikatsiyadan foydalanmoqda. Springer (2025) tadqiqotiga ko'ra, IoT xavfsizlik zaifliklarining muhim qismi kriptografiyaning noto'g'ri yoki umuman qo'llanilmasligi bilan bog'liq.

---

### 3.2 Zaif Autentifikatsiya va Default Parollar Muammosi

IoT qurilmalarining tarmoqqa ulanganda o'zini to'g'ri identifikatsiya qila olmasligi — eng keng tarqalgan xavfsizlik muammolaridan biri. Asosiy ko'rinishlari:

1. **Default (standart) parollar:** Ko'plab ishlab chiqaruvchilar qurilmalarni `admin/admin`, `root/root`, `1234` kabi zaif parollar bilan yetkazib beradi va foydalanuvchilar ularni o'zgartirishni unutadi.
2. **Hard-coded credentials:** Ba'zi firmware larda autentifikatsiya ma'lumotlari dastur kodiga "o'chirib bo'lmaslik" bilan yozib qo'yiladi.
3. **Bir tomonlama autentifikatsiya:** Faqat foydalanuvchi qurilmani tekshiradi, qurilma esa serverning haqiqiyligini tekshirmaydi — bu Man-in-the-Middle hujumlariga yo'l ochadi.

Mirai botnet hujumi aynan shu zaiflikdan foydalangan: zararli dastur 61 ta keng tarqalgan default login/parol kombinatsiyasini sinab, millionlab IP kameralar va router larni o'z nazoratiga olgan [ResearchGate, 2017; CISA, 2016].

**Yechim yo'nalishlari:**
- **X.509 sertifikatlar** asosidagi o'zaro autentifikatsiya
- **PUF (Physical Unclonable Function)** — qurilmaning noyob fizik xususiyatlariga asoslangan identifikatsiya [Springer, 2023]
- **Zero-Touch Provisioning** — qurilma dastlabki ulanishda avtomatik ravishda xavfsiz kalitlarni olishi
- **TPM (Trusted Platform Module)** — apparat darajasida kalit saqlash

NIST SP 800-213 (2021) federal tashkilotlar uchun IoT qurilmalari autentifikatsiyasiga qo'yiladigan minimal talablarni belgilaydi [NIST SP 800-213, 2021].

---

### 3.3 Xavfsiz Firmware Yangilash (OTA Security) Muammosi

IoT qurilmalari uzoq vaqt — ba'zan 10–15 yil — ishlatiladi. Bu davrda zaifliklari topilgan firmware ni yangilash jarayoni xavfsizlik nuqtai nazaridan kritik ahamiyat kasb etadi.

**Asosiy muammolar:**
- Ko'plab arzon IoT qurilmalari OTA (Over-the-Air) yangilash funksiyasiga umuman ega emas.
- Yangilash kanali shifrlanmagan bo'lsa, **rollback hujum** (eski, zaif versiyaga qaytarish) amalga oshirilishi mumkin.
- Yangilash paketining imzosi tekshirilmasa, **firmware almashtirish hujumi** amalga oshirilishi mumkin.

**Yechim standartlari:**
- **Code signing** (ECDSA yoki RSA) orqali firmware autentifikatsiyasi
- **TLS/DTLS** orqali shifrlangan yangilash kanali
- **Rollback protection** — qurilma eski versiyani qabul qilmasligi uchun versiya sanasi yoki counter bilan himoya
- **SUIT (Software Updates for Internet of Things)** — IETF tomonidan ishlab chiqilgan standart format [IETF RFC 9124, 2022]

---

### 3.4 DDoS va Botnet Hujumlari

IoT qurilmalari yirik **botnet** tarmog'ining qurilish materiali sifatida faol ishlatilmoqda. Buning sabablari:
- Doimo internetga ulangan holat
- Zaif autentifikatsiya (yuqorida ko'rib o'tildi)
- Hisoblash resurslari hujum uchun yetarli, lekin himoya uchun kam

**Mirai (2016)** — tarixiy nuqtai nazardan eng muhim IoT botnet hujumi. OVH internet-provayderiga qaratilgan hujumda **1,1–1,5 Tbit/s** quvvatdagi DDoS qayd etildi — o'sha paytdagi rekord [CISA, 2016; Cloudflare, 2017]. Mirai dan keyin uning manba kodi ommaga chiqarildi va natijada **Okiru, Masuta, PureMasuta, Hajime** kabi o'nlab variantlari paydo bo'ldi [ResearchGate, 2020].

ENISA Threat Landscape 2024 hisobotiga ko'ra, IoT qurilmalariga yo'naltirilgan hujumlar hajmi va murakkabligi yil sayin o'smoqda va ular kibertahdid landshaftining ustuvor muammolaridan biri sifatida tasniflanmoqda [ENISA ETL, 2024].

**Yumshatish choralarining asosiy yo'nalishlari:**
- Tarmoq segmentatsiyasi (IoT qurilmalarini alohida VLAN ga joylashtirish)
- Anomaliya aniqlash tizimlari (qurilma o'zi uchun g'ayritabiiy trafik naqshlarini monitoring qilish)
- ISP darajasida trafik filtrlash
- **Ingress/Egress filtering** — BCP 38 standartiga muvofiq spoofed IP larni bloklash

---

### 3.5 Fizik Xavfsizlik va Side-Channel Hujumlari

An'anaviy IT tizimlaridan farqli ravishda, IoT qurilmalari ko'pincha jismonan keng kirish imkoniyati mavjud muhitlarda joylashadi: ko'chalar, omborlar, sanoat obyektlari. Bu holat fizik hujumlarga yo'l ochadi.

**Asosiy hujum turlari:**

| Hujum turi | Tavsif | Misol |
|-----------|--------|-------|
| **Cold Boot Attack** | Qurilma xotirasidan kalitlarni o'qish | RAM dan AES kalit ajratib olish |
| **Power Analysis (SPA/DPA)** | Energiya sarfi naqshlaridan kriptografik sirlar chiqarish | Kuchlanish kuzatuvi orqali kalit hisoblash |
| **Electromagnetic Analysis** | Elektromagnit nurlanish orqali ma'lumot olish | EM zond bilan signal tutish |
| **Fault Injection** | Qurilmaga sun'iy xato kiritish | Lazer bilan bit o'zgartirish |
| **JTAG/UART Debug Interface** | Debug portlar orqali firmware o'qish | Dasturlash portlariga direkt ulanish |

**Himoya choralari:**
- Debug interfeyslarini ishlab chiqarishda o'chirish
- **Tamper-evident packaging** — ochilganda zararlanuvchi qoplamalar
- Kriptografik operatsiyalarni apparat darajasida izolyatsiya qilish (**TEE** — Trusted Execution Environment)
- **PUF** — Side-channel hujumlariga bardoshli noyob qurilma identifikatori

---

### 3.6 Maxfiylik va Ma'lumotlar Himoyasi Muammolari

IoT qurilmalari ulkan hajmda shaxsiy ma'lumotlarni to'playdi: sog'liq ko'rsatkichlari (smart hours, tibbiy sensorlar), joylashuv (GPS), uy hayoti (smart kameralar, ovoz yozuvchilari), moliyaviy odatlar (smart to'lov tizimlari).

**Asosiy muammolar:**

1. **Data minimization printsipining buzilishi:** Ko'plab qurilmalar zaruriyatidan ortiq ma'lumot to'playdi va saqlaydi.
2. **Bulut saqlashning xavfliligi:** Ma'lumotlar ishlab chiqaruvchi serverlariga yuboriladi va foydalanuvchi nazoratidan chiqadi.
3. **Sensorlar o'rtasida ma'lumot qo'shilishi (data fusion):** Alohida zararsiz ko'ringan ma'lumotlar birgalikda juda maxfiy profil hosil qilishi mumkin (masalan: uyqy soatlari + ovoz faolligi + elektr sarfi = uy ichidagi yashash naqshlari).
4. **Umumiy maxfiylik qoidalariga mos kelmaslik:** GDPR (Yevropa), CCPA (Kaliforniya) kabi qoidalar IoT ishlab chiqaruvchilariga og'ir talablar qo'yadi.

Springer (2025) tadqiqoti ko'rsatadiki, IoT bilan bog'liq maxfiylik muammolari faqat texnik jihat emas — ular to'g'ri foydalanuvchilarning xatti-harakatiga ham bog'liq va ijtimoiy-texnik yondashuv talab etadi.

---

### 3.7 Tarmoq Protokollarining Xavfsizlik Zaif Tomonlari

IoT tizimlarida qo'llaniladigan maxsus tarmoq protokollari ham o'ziga xos zaifliklar keltirib chiqaradi:

| Protokol | Qo'llanilishi | Asosiy zaifliklar |
|---------|--------------|-------------------|
| **MQTT** | M2M kommunikatsiya | Default holda shifrlanmagan; broker autentifikatsiya zaif; wildcard topic subscription hujumlari |
| **CoAP** | Resource-constrained qurilmalar | UDP asosida — spoof qilish oson; DTLS qo'llanilmasa ma'lumotlar ochiq |
| **Zigbee** | Smart home | Key transport zaif; replay hujumlar; klonlash |
| **BLE** | Wearable, medical devices | Pairing jarayonida MITM; fingerprinting hujumlari |
| **Z-Wave** | Smart home | Shifrlash sxemasi zaif variantlarda |

MQTT protokolining xavfsizligi, ayniqsa, katta nazoratga sazovor bo'lgan — o'rta hujum (MITM) va message spoofing hujumlari amalda namoyish etilgan [IEEE, 2024; Springer, 2024].

---

## 4. HUJUMLAR TASNIFI VA STATISTIKA

ENISA Threat Landscape 2023 hisobotida 2022 yil iyul — 2023 yil iyun oralig'ida **2580 ta kiberincident** qayd etilgan, shulardan muhim qismi IoT infratuzilmasiga qaratilgan [ENISA ETL, 2023].

IoT ga yo'naltirilgan asosiy hujum vektori turlari:

```
┌─────────────────────────────────────────────────────────┐
│              IoT HUJUM VEKTORLARI TASNIFI               │
├─────────────────┬───────────────────────────────────────┤
│ QATLAM          │ HUJUM TURLARI                          │
├─────────────────┼───────────────────────────────────────┤
│ Qurilma         │ Fizik manipulyatsiya, side-channel,    │
│ (Hardware)      │ fault injection, debug port exploiting │
├─────────────────┼───────────────────────────────────────┤
│ Firmware /      │ Firmware almashtirish, buffer overflow,│
│ Dasturiy ta'min │ hardcoded credentials, insecure OTA   │
├─────────────────┼───────────────────────────────────────┤
│ Tarmoq          │ MITM, replay, DDoS, traffic analysis, │
│                 │ protocol exploitation (MQTT, CoAP)     │
├─────────────────┼───────────────────────────────────────┤
│ Ilova /         │ API zaifliklar, autentifikatsiya bypass,│
│ Bulut           │ insecure storage, data leakage         │
└─────────────────┴───────────────────────────────────────┘
```

---

## 5. MAVJUD YECHIMLAR VA STANDARTLAR

### 5.1 Xalqaro Standartlar va Yo'riqnomalar

| Standart | Tashkilot | Mazmun |
|---------|----------|--------|
| **NIST SP 800-213** | NIST (AQSh) | Federal tashkilotlar uchun IoT qurilmalar kiberxavfsizligi bo'yicha yo'riqnoma [2021] |
| **NIST SP 800-232** | NIST (AQSh) | Resurs-cheklangan qurilmalar uchun ASCON-asosli engil kriptografiya standarti [2025] |
| **ETSI EN 303 645** | ETSI (Yevropa) | Iste'mol IoT qurilmalari uchun kiberxavfsizlik asoslari |
| **ITU-T Y.2060** | ITU | IoT uch qatlamli arxitektura tavsifi |
| **IETF RFC 9124** | IETF | IoT uchun firmware yangilash arxitekturasi (SUIT) [2022] |
| **IEC 62443** | IEC | Sanoat avtomatlashtirish va nazorat tizimlari xavfsizligi |

### 5.2 Texnik Yechimlar

**Kriptografiya sohasida:**
- ASCON (NIST LWC Standard) — IoT uchun tasdiqlangan engil kriptografiya
- **DTLS (Datagram TLS)** — UDP asosidagi xavfsiz kommunikatsiya protokoli
- **ECDH** va **ECDSA** — past resursli qurilmalar uchun elliptic curve kriptografiyasi

**Autentifikatsiya sohasida:**
- PUF — apparat darajasidagi noyob qurilma identifikatori
- TPM — ishonchli platforma moduli
- Zero-Touch Provisioning — qurilmalarni xavfsiz massiv ulash mexanizmi

**Tarmoq himoyasi sohasida:**
- Tarmoq segmentatsiyasi va mikrosegmentatsiya
- **Zero-Trust Architecture** — "hech kimga ishonma, hammani tekshir" printsipiga asoslangan model
- **SDN (Software-Defined Networking)** asosida dinamik politika boshqaruvi

**Monitoring va aniqlash sohasida:**
- ML-asosli anomaliya aniqlash tizimlari
- **IDS/IPS** ni IoT segmentlari uchun moslash
- **Honeypot** tarmoqlari — hujumlarni erta aniqlash

---

## 6. MUNOZARA VA KELAJAKDAGI TADQIQOT YO'NALISHLARI

Hozirgi ilmiy adabiyot tahlili bir qator dolzarb ochiq masalalarni ko'rsatmoqda:

### 6.1 Edge Computing va Zero-Trust

Bulut-markazlashgan IoT modelidan **Edge Computing** ga o'tish xavfsizlik arxitekturasini qayta ko'rib chiqishni talab qiladi. Ma'lumot qurilmaga yaqin qayta ishlansa, yangi hujum yuzalari paydo bo'ladi. **Zero-Trust** printsipini IoT/Edge muhitida amalga oshirish — aktiv tadqiqot sohasi.

### 6.2 Suni Intellekt va IoT Xavfsizligi

ML/DL usullari IoT trafigida anomaliyalarni aniqlash, zararli qurilmalarni erta topish va tahdidlarni bashorat qilish uchun tobora ko'proq qo'llanilmoqda. Springer (2024) tadqiqoti XAIoT (eXplainable AI for IoT) kontseptsiyasini taqdim etadi — bu yo'nalish kelajakda axborot xavfsizligi auditida muhim o'rin egallashi kutilmoqda.

### 6.3 Kvant Hisoblash Tahdidi

Post-kvant kriptografiya IoT uchun alohida muammo — mavjud PKI va RSA/ECC asosidagi tizimlar kvant kompyuterlari kuchayishi bilan zaif bo'lishi mumkin. NIST post-kvant kriptografiya standartlarini ishlab chiqmoqda, ammo ularni resurs-cheklangan IoT qurilmalariga tatbiq etish qo'shimcha muhandislik yechimlarini talab qiladi.

### 6.4 Tartibga Solish va Qonunchilik Muammolari

Texnik yechimlar yetarli emas — **minimal xavfsizlik talablarini qonun bilan majburiy qilish** zarurligi ortib bormoqda. AQShda **IoT Cybersecurity Improvement Act (2020)**, Yevropada **EU Cyber Resilience Act (2024)** kabi qonunlar bu yo'nalishda muhim qadam bo'ldi. O'zbekiston va boshqa rivojlanayotgan mamlakatlar uchun ushbu tajribani mahalliy kontekstda o'rganish — dolzarb tadqiqot yo'nalishi.

---

## 7. XULOSA

Ushbu maqolada IoT tizimlaridagi xavfsizlik muammolari quyidagi asosiy yo'nalishlar bo'yicha tahlil qilindi:

1. **Resurs cheklanishi** — lightweight kriptografiyani talab qiladi; NIST ASCON standarti (SP 800-232) bu muammoga asosiy texnik javob hisoblanadi.
2. **Zaif autentifikatsiya** — default parollar va sertifikatsiyasiz qurilmalar hali ham keng tarqalgan; PUF va X.509 asosidagi yechimlar kelajak yo'li.
3. **Xavfsiz OTA yangilash** — SUIT/IETF standarti va code signing lozim.
4. **DDoS va botnet hujumlari** — Mirai va uning avlodlari IoT infratuzilmasiga sistemik tahdid bo'lib qolmoqda; tarmoq segmentatsiyasi va anomaliya aniqlash asosiy choralar.
5. **Fizik xavfsizlik** — side-channel hujumlar va debug port exploiting real tahdid; TEE va PUF texnologiyalari yechim sifatida ko'rib chiqilmoqda.
6. **Maxfiylik** — ma'lumotlar minimalligi va foydalanuvchi nazorati — texnik va normativ choralarni bir vaqtda talab qiladi.
7. **Protokol zaifliklar** — MQTT, CoAP, Zigbee kabi IoT protokollarining xavfsizligini kuchaytirish davom etmoqda.

IoT xavfsizligi — bu faqat texnik masala emas, u tartibga solish, foydalanuvchi xulqi va arxitektura qarorlarining murakkab kesishmasida joylashgan ko'p qirrali muammo. Kelgusi tadqiqotlar uchun Zero-Trust/Edge integratsiyasi, post-kvant IoT kriptografiyasi va ML-asosli real vaqt anomaliya aniqlash tizimlari ustuvor yo'nalishlar sifatida tavsiya etiladi.

---

## ADABIYOTLAR

1. **Statista** (2025). *IoT connections worldwide 2034.* https://www.statista.com/statistics/1183457/global-iot-market-size/

2. **NIST** (2023). *NIST Selects 'Lightweight Cryptography' Algorithms to Protect Small Devices.* https://www.nist.gov/news-events/news/2023/02/nist-selects-lightweight-cryptography-algorithms-protect-small-devices

3. **NIST SP 800-232** (2025). *Ascon-Based Lightweight Cryptography Standards for Constrained Devices.* https://csrc.nist.gov/pubs/sp/800/232/final

4. **NIST SP 800-213** (2021). *IoT Device Cybersecurity Guidance for the Federal Government.* https://csrc.nist.gov/publications/detail/sp/800-213/final

5. **CISA** (2016). *Heightened DDoS Threat Posed by Mirai and Other Botnets.* https://www.cisa.gov/news-events/alerts/2016/10/14/heightened-ddos-threat-posed-mirai-and-other-botnets

6. **Cloudflare** (2017). *Inside the infamous Mirai IoT Botnet — A Retrospective Analysis.* https://blog.cloudflare.com/inside-mirai-the-infamous-iot-botnet-a-retrospective-analysis/

7. **ENISA** (2024). *ENISA Threat Landscape 2024 (July 2023 – June 2024).* https://www.enisa.europa.eu/publications/enisa-threat-landscape-2024

8. **ENISA** (2023). *ENISA Threat Landscape 2023.* https://www.enisa.europa.eu/publications/enisa-threat-landscape-2023/

9. **Springer** (2025). *Internet of Things (IoT) applications security trends and challenges.* https://link.springer.com/article/10.1007/s43926-024-00090-5

10. **Springer** (2025). *A survey on IoT security: challenges and their solutions using machine learning and blockchain technology.* https://link.springer.com/article/10.1007/s10586-025-05208-0

11. **Springer** (2024). *A survey on IoT application layer protocols, security challenges, and the role of explainable AI in IoT (XAIoT).* https://link.springer.com/article/10.1007/s10207-024-00828-w

12. **Springer** (2023). *Device-specific security challenges and solution in IoT edge computing: a review.* https://link.springer.com/article/10.1007/s11227-023-05450-6

13. **Springer** (2023). *Cybersecurity challenges in IoT-based smart renewable energy.* https://link.springer.com/article/10.1007/s10207-023-00732-9

14. **IEEE Xplore** (2016). *Internet of things (IoT) security: Current status, challenges and prospective measures.* https://ieeexplore.ieee.org/document/7412116

15. **IEEE Xplore** (2019). *Demystifying IoT Security: An Exhaustive Survey on IoT Vulnerabilities.* https://ieeexplore.ieee.org/document/8688434

16. **ResearchGate** (2024). *A Survey on Security, Privacy, Trust, and Architectural Challenges in IoT Systems.* https://www.researchgate.net/publication/379388970

17. **ResearchGate / Antonakakis et al.** (2017). *Understanding the Mirai Botnet.* USENIX Security Symposium 2017. https://www.researchgate.net/publication/363475096

18. **arXiv** (2023). *First Work Implementing the NIST Cryptographic Standard ASCON.* https://arxiv.org/abs/2306.08178

19. **IETF RFC 9124** (2022). *A Manifest Information Model for Firmware Updates in Internet of Things (IoT) Devices.* https://www.rfc-editor.org/rfc/rfc9124

20. **ResearchGate / Kolias et al.** (2017). *DDoS in the IoT: Mirai and other botnets.* IEEE Computer, 50(7), 80-84. https://www.researchgate.net/publication/318288727

---

*Maqola 2026 yil iyun oyi holatiga ko'ra tuzilgan. Barcha havolalar tekshirilgan.*
*Content was rephrased and structured for academic compliance.*
