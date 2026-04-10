<div align="center">

# 🌪️ DEBT Manager — API Integracyjne TORNADO

**Kompletna dokumentacja techniczna API integracyjnego systemu DEBT Manager**

*Importy danych • Kolejki komunikatów • Monitorowanie statusów • Integracja bez tarcia*

[![Docs](https://img.shields.io/badge/docs-online-ffaa00?style=for-the-badge&logo=readthedocs&logoColor=white)](https://oskar-bialek-bakk.github.io/intrum-integration-api-docs/)
[![MkDocs Material](https://img.shields.io/badge/MkDocs-Material-3e3e3e?style=for-the-badge&logo=materialformkdocs&logoColor=white)](https://squidfunk.github.io/mkdocs-material/)
[![BAKK](https://img.shields.io/badge/by-BAKK-e69b05?style=for-the-badge)](https://bakk.com)

</div>

---

## ✨ O dokumentacji

**DEBT Manager API Integracyjne** (kryptonim **TORNADO**) to zestaw usług, który pozwala kontrahentom zasilać system **bez dotykania UI** — bezpośrednio z własnych pipeline'ów ETL, procesów windykacyjnych czy zewnętrznych baz danych.

Ta dokumentacja to **jedno źródło prawdy** dla całej integracji:

- 📘 **Czytelna jak książka** — zamiast rozproszonego Confluence i Worda masz spójne, przeszukiwalne podręczniki.
- 🎯 **Precyzyjna jak spec** — każdy endpoint, każde pole komunikatu, każdy status opisany z przykładami JSON.
- 🚀 **Praktyczna jak tutorial** — gotowe przewodniki „jak zaimportować", „jak zasilić", „jak dodać nowy typ importu".
- 🎨 **Nowoczesny design** — dark mode, brandowe kolory BAKK, pełna responsywność, kopiowalne snippety kodu.

---

## 🌐 Dokumentacja online

> ### 👉 **[oskar-bialek-bakk.github.io/intrum-integration-api-docs](https://oskar-bialek-bakk.github.io/intrum-integration-api-docs/)**

Strona budowana **automatycznie** przy każdym pushu — MkDocs Material + GitHub Pages, zero ręcznej roboty.

---

## 📚 Zawartość

<table>
<tr>
<td width="50%" valign="top">

### 🧭 [Założenia](docs/zalozenia/)
Od czego zacząć: **architektura fizyczna**, konfiguracja kontrahenta, procedura importów, kolejki RabbitMQ. Wszystko, co musisz wiedzieć, zanim napiszesz pierwszy request.

</td>
<td width="50%" valign="top">

### 🔌 [Funkcje API](docs/funkcje-api/)
**13 endpointów** pokrywających pełen cykl życia importu: upload pliku, kolejkowanie komunikatów, śledzenie statusu, słowniki walidacyjne, e-maile odbiorców.

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 💬 [Komunikaty](docs/komunikaty/)
**14 typów komunikatów** — `Case`, `Customer`, `Contract`, `Payment`, `Action` i inne. Każdy z pełnym drzewem pól, macierzą flag `ObjectsUpdateBehaviour` i gotowymi przykładami JSON.

</td>
<td width="50%" valign="top">

### 🎬 [Przypadki użycia](docs/przypadki-uzycia/)
Praktyczne scenariusze krok po kroku: **import z pliku testowego**, **zasilenie bez pliku**, **konfiguracja nowego typu importu**. Od zera do działającej integracji.

</td>
</tr>
</table>

---

## 🚀 Lokalne uruchomienie

Chcesz podejrzeć dokumentację lokalnie przed pushem? Jeden skrypt, dwa polecenia:

```bash
pip install -r requirements.txt
mkdocs serve
```

Dokumentacja wystartuje pod **`http://127.0.0.1:8000`** z live-reload — zmieniasz plik, strona odświeża się automatycznie. ⚡

---

## 🗂️ Struktura repozytorium

```
📁 integration-api/
├── 📄 mkdocs.yml              # Konfiguracja MkDocs Material (theme, nav, plugins)
├── 📄 requirements.txt        # Zależności Pythona
├── 📁 docs/                   # ← Źródłowe pliki Markdown (edytujesz tutaj)
│   ├── 📁 zalozenia/          # Architektura, konfiguracja, procedura
│   ├── 📁 funkcje-api/        # Opisy 13 endpointów API
│   │   └── 📁 importy/
│   ├── 📁 komunikaty/         # 14 specyfikacji komunikatów
│   ├── 📁 przypadki-uzycia/   # Przewodniki how-to
│   ├── 📁 diagrams/           # Diagramy draw.io (architektura, flow)
│   ├── 📁 images/             # Logo, favicon, zrzuty ekranu
│   └── 📁 stylesheets/        # Customowy CSS (branding BAKK)
└── 📁 sync/                   # Skrypty synchronizacji do Confluence on-premise
```

---

## 🎨 Design

Dokumentacja korzysta z **brandowych kolorów BAKK**:

- 🟠 **Amber** `#ffaa00` — akcent, hover, highlighty
- 🟡 **Dark gold** `#e69b05` — linki, hero
- ⚫ **Charcoal** `#3e3e3e` — header, tło nawigacji
- 🔵 **Steel blue-gray** `#9db3c8` — linki w dark mode

Czcionki: **Source Sans 3** (tekst) + **JetBrains Mono** (kod). Dark mode domyślnie, light mode dostępny jednym kliknięciem ☀️/🌙 w headerze.

---

<div align="center">

**Zbudowane z 🧡 przez zespół [BAKK](https://bakk.com)**

*Masz pytanie, sugestię, znalazłeś błąd? Otwórz issue albo zgarnij link do odpowiedniej strony w dokumentacji i daj znać.*

</div>
