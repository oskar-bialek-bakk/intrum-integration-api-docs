---
title: "How-to: Zaimportowanie przykładowego pliku importowego z użyciem aplikacji testowej"
---

W celu prezentacji działania API integracyjnego dostawca  przekazuje kod źródłowy testowej aplikacji IntegrationsApiTestAPP, która pokazuje na przykładzie języka programowania C# w jaki sposób można wykonać import nowych spraw do systemu DEBT Manager z pliku Excel.

!!! info
    Źródła aplikacji testowej dostępne są w repozytorium: [Dm.Demo.IntegrationsApiTestAPP](https://bakkspzoo@dev.azure.com/bakkspzoo/Dm.Demo/_git/Dm.Demo.IntegrationsApiTestAPP)

W celu zaimportowania przykładowego pliku importowego z użyciem aplikacji testowej należy:

1) Uzupełnić w pliku domyślne parametry znajdują się app.config/IntegrationsApiTestAPP.exe.config parametry przykładowego importu spraw:

-   OrginalCreditorId - kontrahent w ramach którego zostaną zaimportowane sprawy
-   PortfolioId - portfel kontrahenta w ramach którego zostaną zaimportowane sprawy
-   ActionTypeId - akcja która wyznaczy etap dla nowo zaimportowanych spraw
-   DocumentTypeId - typ dokumentu księgowego do którego zostanie załadowane zadłużenie

2) Zbudować kod źródłowy aplikacji IntegrationsApiTestAPP

3) Uruchomić aplikację a następnie podać parametry konfiguracyjne autentykacji aplikacji (domyślne parametry znajdują się app.config/IntegrationsApiTestAPP.exe.config)

-   API Host - adres API Integracyjnego
-   Serwer autoryzacji - adres serwera autoryzacji

4) Kliknąć przycisk "Choose import file ..." i wybrać jeden z dostarczonych z aplikacją plików z testowym importem:

-   ErrorCaseSample.xlsx - plik z niepoprawnym importem zawierającym błędy walidacji danych
-   OkCaseSample.xlsx - plik z poprawnym importem danych, powodującym założenie nowej sprawy w systemie DEBT Manager

5) Zaimportować wybrany plik klikając przycisk "Execute Test scenario"

6) Obserwować wynik kolejnych kroków importu w oknie Test output, opcjonalnie prześledzić w debuggerze wykonanie kolejnych kroków kodu źródłowego klasy przykładowego importu - CaseImport.cs
