---
title: "ContractBalance"
---

**Opis:**

Dla wypełnionej flagi **IncludeContractBalance** = **true**

Komunikat umożliwia aktualizację stanu finansowego wierzytelności poprzez nadpisanie dokumentów pojedyńczą wartością salda. Zachowanie komunikatu zależne od flagi „Czy zaniechaj”
Jeśli flaga wypełniona na tak to:
1\. Zostaje dodane(ew.nadpisane jeśli wcześniej już jakieś było) księgowanie na wskazanym dokumencie księgowym, na wskazanym koncie księgowym – kwota i waluta zgodna z przekazaniem
2\. Zaległości aktualne z pozostałych dokumentów tej wierzytelności są przeksięgowywane na typ Umorzenie/zaniechanie
W efekcie aktualna kwota do windykacji takiej wierzytelności będzie zgodna ze wskazaniem nowo otrzymanej kwoty

Jeśli flaga „Czy zaniechaj” nie jest wypełniona na tak wówczas:
1\. Zostaje dodany (ew.nadpisany) nowy dokument wskazanego typu wraz z księgowaniem nowej kwoty.
2\. Pozostałe dokumenty wraz z księgowaniami na wierzytelności pozostają niezmienne.
W efekcie po takiej operacji aktualna kwota do windykacji będzie sumą otrzymanej kwoty z kwotą do windykacji sprzed operacji.

Dla wypełnionej flagi **CloseOpenedCases** = **true**

Drugim możliwym użyciem konunikatu jest zamknięcie spraw windykacyjnych powiązanych. Zamknięcie jest uzależnone od wartości flagi IncludeContractBalance. Jest to flaga mówiąca o tym, czy należy zamknąć aktualnie otwarte sprawy windykacyjne powiązane przez Match Key.
1\. Ustawiona na true zamykałaby wszystkie otwarte sprawy windykacyjne powiązane ze zmienianą wierzytelnością.
2\. Ustawiona na false nie zmienia stanu spraw (nie otwiera zamkniętych spraw).



**Lokalizacja w bazie integracyjnej:** dm\_messages.contractbalance\_details

**Wykorzystanie pól komunikatu:**

| NAZWA POLA | TYP DANYCH | WYMAGALNOŚĆ | OPIS | LOKALIZACA W BAZIE DM |
| --- | --- | --- | --- | --- |
| MatchKey | string | Tak | Patrz rozdział Komunikaty → Budowa komunikatów | wierzytelnosc.wi_ext_id |
| MatchKeyType | int | Tak | Patrz rozdział Komunikaty → Budowa komunikatów. Dopuszczalne wartości:3 - zewnętrzny numer umowy (External contract number wierzytelnosc.wi_ext_id) | Brak |

Przykład 1 (zmiana salda)

```json

```



Przykład 2 (zamknięcie spraw)

```json

```
