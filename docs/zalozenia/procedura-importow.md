---
title: "Procedura importów danych od kontrahenta"
---

Import danych od kontrahenta składa się z następujących kroków, która bardziej szczegółowo prezentuje poniższy diagram:

```mermaid
flowchart LR
    A["1. Kontrahent wystawia dane\nna udział sieciowy"] --> B["2. IT monitoruje udział sieciowy\ni pobiera plik od kontrahenta"]
    B --> C["3. IT importuje plik\nprzy użyciu API"]
    C --> D["Integration API\nprzyjmuje i kolejkuje\nkomunikaty"]
    D --> E["Worker Service\nprzetwarza komunikaty\nz kolejek"]
    E --> F["Dane zaimportowane\ndo DEBT Manager"]

    style A fill:#4a9eff,color:#fff
    style B fill:#c061cb,color:#fff
    style C fill:#c061cb,color:#fff
    style D fill:#26a269,color:#fff
    style E fill:#26a269,color:#fff
    style F fill:#26a269,color:#fff
```

**Legenda:** :blue_square: Kontrahent | :purple_square: IT | :green_square: System

1) Kontrahent wystawia dane na udział sieciowy

2) IT monitoruje udział sieciowy i pobiera plik od kontrahenta

3) IT importuje plik przy użyciu API (techniczna specyfikacja tego kroku znajduje się w rozdziale [Funkcje API > Importy](../funkcje-api/importy/index.md))
