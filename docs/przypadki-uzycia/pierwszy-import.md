---
title: "Pierwszy import"
---

# Pierwszy import API-2-API

<div class="api-section" markdown>
<div class="api-section-title">Opis</div>

Przewodnik krok po kroku pokazuje pełny cykl pierwszego importu wykonywanego w trybie API-2-API. Przykład: import nowego klienta (Customer) wraz z jedną sprawą (Case) i kontraktem (Contract) — 3 komunikaty w trzech kolejkach.

W trybie API-2-API każdy import — niezależnie od wielkości — przebiega według tego samego schematu. Punktowa korekta jednej wpłaty i wsad 10 000 spraw używają tych samych endpointów; różnią się tylko liczbą wywołań [EnqueueImportMessage](../funkcje-api/importy/enqueue-import-message.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 1 — Pobranie tokenu</div>

```bash
curl -X POST https://dmlogin-intrum-dev.groupad1.com/connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=integration&client_secret={CLIENT_SECRET}&scope=api1&grant_type=client_credentials"
```

Z odpowiedzi pobierz `access_token` i wykorzystaj w nagłówku `Authorization: Bearer <access_token>` we wszystkich kolejnych wywołaniach. Szczegóły — [Autoryzacja](../zalozenia/autoryzacja.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 2 — Utworzenie importu</div>

```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/CreateImport \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "ImportTypeId": 100,
    "CreditorId": 1,
    "PortfolioId": 1,
    "ExternalReference": "tutorial-pierwszy-import"
  }'
```

Odpowiedź zawiera `importId`:
```json
{
  "ImportId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
  "ImportTypeName": "DM Integration API Sample - External Payments Import"
}
```

Zapisz wartość `ImportId` — będzie używana w każdym kolejnym kroku. Szczegóły — [CreateImport](../funkcje-api/importy/create-import.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 3 — Zgłoszenie startu fazy Adding</div>

```bash
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "Stage": 1,
    "StageStatus": 1
  }'
```

Szczegóły — [SetImportStep](../funkcje-api/importy/set-import-step.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 4 — (Opcjonalnie) Słowniki i reguły walidacji</div>

Klient API może pobrać słowniki walidacyjne aby zwalidować dane przed wysłaniem:

```bash
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetDictionaries" \
  -H "Authorization: Bearer {TOKEN}"
```

Oraz zarejestrować reguły walidacji egzekwowane przez API (sprawdzane per komunikat w trakcie etapu Consumption; krytyczne reguły domyślne działają zawsze, także bez rejestracji):

```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportValidations \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "ImportId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "Rules": [
      { "Id": 3001, "Level": 3 }
    ]
  }'
```

Szczegóły — [GetDictionaries](../funkcje-api/importy/get-dictionaries.md), [SetImportValidations](../funkcje-api/importy/set-import-validations.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 5 — Wysłanie komunikatów</div>

Wysyłamy 3 komunikaty w kolejności **Customer → Contract → Case** — każdy w osobnym wywołaniu `EnqueueImportMessage`. Pole `message` to **string JSON** (lista obiektów serializowana).

Kolejność ma znaczenie: Case linkuje klienta i wierzytelność po MatchKey, więc Customer i Contract muszą zostać wysłane wcześniej.

**1. Customer:**
```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "queueName": "Customer",
    "message": "[{\"ObjectId\": \"78599fcf-6f66-40e4-b70e-a1345674ebf4\", \"MatchKey\": \"990000001\", \"MatchKeyType\": 2, \"DebtorPortfolioId\": 1, \"ExternalId\": \"990000001\", \"CustomerTypeId\": 1, \"FirstName\": \"Jan\", \"LastName\": \"Kowalski\", \"PESEL\": \"85061504513\"}]"
  }'
```

**2. Contract:**
```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "queueName": "Contract",
    "message": "[{\"ObjectId\": \"0481782f-712e-4a12-8eab-9fa5dace95ee\", \"MatchKey\": \"880000001\", \"MatchKeyType\": 3, \"Number\": \"UM/2024/001\", \"Title\": \"Umowa pożyczki 2024/001\", \"OrginalCreditorId\": 1, \"PortfolioId\": 1}]"
  }'
```

**3. Case:**
```bash
curl -X POST https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/EnqueueImportMessage \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "importId": "2fa859e9-8479-4c7e-b1bb-c85f90f2402c",
    "queueName": "Case",
    "message": "[{\"ObjectId\": \"a570d857-fb10-4d46-aaca-8f4707d4657f\", \"MatchKey\": \"770000001\", \"MatchKeyType\": 6, \"ExternalCaseNumber\": \"770000001\", \"StageAction\": {\"ActionTypeId\": 8}, \"CustomerContractList\": {\"Objects\": [{\"CustomerRoleId\": 1, \"CustomerMatchKey\": \"990000001\", \"CustomerMatchKeyType\": 2, \"ContractMatchKey\": \"880000001\", \"ContractMatchKeyType\": 3}], \"ObjectsUpdateBehaviour\": 6}}]"
  }'
```

Każde wywołanie zwraca status (`Success` / `Warning` / `Error`). Szczegóły struktury komunikatów — [Komunikaty](../komunikaty/index.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 6 — Zamknięcie fazy</div>

**Wariant A — Walidacja po stronie API** (domyślnie):
```bash
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "Stage": 1,
    "StageStatus": 2
  }'
```

API automatycznie uruchamia Stage 3 Validation z regułami z Kroku 4.

**Wariant B — Walidacja po stronie Klienta API** (gdy Klient API zwalidował dane u siebie przed wysłaniem):
```bash
curl -X POST "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/SetImportStep?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c" \
  -H "Authorization: Bearer {TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "Stage": 3,
    "StageStatus": 2
  }'
```

API pomija własną walidację i przechodzi bezpośrednio do Stage 4 Consumption. (Uwaga: `Stage=2` to Transformation, ustawiany wyłącznie wewnętrznie przez API.)

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 7 — Monitorowanie statusu</div>

Co kilkanaście sekund polluj status importu:
```bash
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetImportStatus?importId=2fa859e9-8479-4c7e-b1bb-c85f90f2402c&includeMessagesStatus=true" \
  -H "Authorization: Bearer {TOKEN}"
```

Z odpowiedzi sprawdź pole `isFinished`: gdy `true`, import jest zakończony. W `currentStep` widzisz aktualny etap i jego status. W `importMessages` widzisz statusy poszczególnych komunikatów (`status_id`: 2=SUCCESS, 3=ERROR, 5=INPROGRESS).

Jeśli `currentStep.stage_id=3 (Validation)` i `status_id=3 (Error)`, import jest wstrzymany na walidacji. Szczegóły błędów w `currentStep.stepDetailsList`.

Szczegóły — [GetImportStatus](../funkcje-api/importy/get-import-status.md).

</div>

---

<div class="api-section" markdown>
<div class="api-section-title">Krok 8 — (Opcjonalnie) Status pojedynczego komunikatu</div>

Aby sprawdzić status pojedynczego obiektu (np. konkretnej wpłaty), użyj `ObjectId` z komunikatu:

```bash
curl "https://dmapi-intrum-dev.groupad1.com/pl/IntegrationsAPI/import/GetObjectStatusByID?objectId=case-001" \
  -H "Authorization: Bearer {TOKEN}"
```

Szczegóły — [GetObjectStatusByID](../funkcje-api/importy/get-object-status-by-id.md).

</div>
