---
title: "Import pliku testowego"
---

# Import pliku testowego

<div class="api-section" markdown>
<div class="api-section-title">Opis</div>

Przewodnik pokazuje jak zaimportować przykładowy plik Excel do systemu DEBT Manager z użyciem testowej aplikacji **IntegrationsApiTestAPP** (C#).

</div>

!!! info "Repozytorium aplikacji testowej"
    Źródła aplikacji dostępne są w repozytorium: [Dm.Demo.IntegrationsApiTestAPP](https://bakkspzoo@dev.azure.com/bakkspzoo/Dm.Demo/_git/Dm.Demo.IntegrationsApiTestAPP)

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 1 — Konfiguracja parametrów importu</div>

W pliku `app.config` / `IntegrationsApiTestAPP.exe.config` uzupełnij parametry przykładowego importu spraw:

```json
{
  "OrginalCreditorId": 10013,
  "PortfolioId": 94,
  "ActionTypeId": 1,
  "DocumentTypeId": 1
}
```

<ul class="param-list">
  <li>
    <span class="param-name required">OrginalCreditorId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID kontrahenta, w ramach którego zostaną zaimportowane sprawy.</span>
  </li>
  <li>
    <span class="param-name required">PortfolioId</span>
    <span class="param-type">int</span>
    <span class="param-desc">ID portfela kontrahenta, pod który trafia import.</span>
  </li>
  <li>
    <span class="param-name required">ActionTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Akcja wyznaczająca etap dla nowo zaimportowanych spraw.</span>
  </li>
  <li>
    <span class="param-name required">DocumentTypeId</span>
    <span class="param-type">int</span>
    <span class="param-desc">Typ dokumentu księgowego, do którego zostanie załadowane zadłużenie.</span>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 2 — Budowanie i uruchomienie</div>

1. Zbuduj kod źródłowy aplikacji **IntegrationsApiTestAPP**.
2. Uruchom aplikację i podaj parametry autentykacji (domyślne wartości znajdują się w `app.config`):

<ul class="param-list">
  <li>
    <span class="param-name required">API Host</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adres API integracyjnego.</span>
  </li>
  <li>
    <span class="param-name required">Serwer autoryzacji</span>
    <span class="param-type">string</span>
    <span class="param-desc">Adres serwera autoryzacji.</span>
  </li>
</ul>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 3 — Wybór pliku importowego</div>

Kliknij przycisk **"Choose import file ..."** i wybierz jeden z plików dostarczonych z aplikacją:

<div class="role-card">
  <div class="role-header">
    <span class="role-name">OkCaseSample.xlsx</span>
  </div>
  <p class="role-desc">Poprawny import danych — powoduje założenie nowej sprawy w systemie DEBT Manager.</p>
</div>

<div class="role-card">
  <div class="role-header">
    <span class="role-name">ErrorCaseSample.xlsx</span>
  </div>
  <p class="role-desc">Niepoprawny import — zawiera celowe błędy walidacji danych (do testowania obsługi błędów).</p>
</div>

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 4 — Wykonanie importu</div>

1. Kliknij przycisk **"Execute Test scenario"**.
2. Obserwuj wynik kolejnych kroków importu w oknie **Test output**.
3. Opcjonalnie: prześledź w debuggerze wykonanie kolejnych kroków w klasie `CaseImport.cs`.

</div>
