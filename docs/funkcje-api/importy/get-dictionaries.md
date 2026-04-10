---
title: "GetDictionaries"
---

**Opis:** Funkcja pobiera wartości wszystkich słowników używanych przez Api

**Typ żądania:** GET

**URL żądania:** `https://[adres_api]/Dm/GetDictionaries`

**Parametry żądania:**

| Nazwa | Typ danych | Typ parametru | Opis |
| --- | --- | --- | --- |

**Pola odpowiedzi:**

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| →Name | string | Nazwa tabeli słownikowej w postaci nazwa_schematu.nazwa_tabeli |
| →Rows | złożony | Lista wierszy występujących w słowniku |
| → →Id | int | Id wpisu z tabeli słownikowej |
| → →Name | string | Nazwa wpisu z tabeli słownikowej |

**Przykład odpowiedzi:**

```json
[
  {
    "Name": "dbo.umowa_kontrahent",
    "Rows": [
      {
        "Id": 94,
        "Name": "IT Import Test"
      },
      {
        "Id": 1,
        "Name": "PKL"
      },
      {
        "Id": 2,
        "Name": "VELO"
      }
    ]
  },
  {
    "Name": "dbo.sygnatura_typ",
    "Rows": [
      {
        "Id": 1,
        "Name": "Sądowa"
      },
      {
        "Id": 2,
        "Name": "Komornicza"
      },
      {
        "Id": 3,
        "Name": "Policyjna"
      },
      {
        "Id": 4,
        "Name": "Prokuratorska"
      }
    ]
  },
  {
    "Name": "dbo.wlasciwosc_typ_podtyp_dziedzina",
    "Rows": [
      {
        "Id": 1,
        "Value": "[{\"Name\":\"wtpd_wt_id\",\"Value\":\"1\"},{\"Name\":\"wtpd_dzi_id\",\"Value\":\"1\"},{\"Name\":\"wtpd_wpt_id\",\"Value\":\"1\"}]"
      },
      {
        "Id": 2,
        "Value": "[{\"Name\":\"wtpd_wt_id\",\"Value\":\"1\"},{\"Name\":\"wtpd_dzi_id\",\"Value\":\"2\"},{\"Name\":\"wtpd_wpt_id\",\"Value\":\"1\"}]"
      },
      {
        "Id": 3,
        "Value": "[{\"Name\":\"wtpd_wt_id\",\"Value\":\"1\"},{\"Name\":\"wtpd_dzi_id\",\"Value\":\"3\"},{\"Name\":\"wtpd_wpt_id\",\"Value\":\"1\"}]"
      }
	]
  }
]
```
