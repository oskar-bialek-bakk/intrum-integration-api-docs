---
title: "Kolejki"
---

W ramach API integracyjnego wykorzystywane są kolejki danych, które pełnią następującą role:

-   zapewniają asynchroniczne przetwarzanie danych
-   pozwalają na przeniesienie wybranych elementów przetwarzania danych do systemów zewnętrznych w stosunku do API np. walidacja importu może zostać wykonana przez zewnętrzny system, który pobiera komunikat o konieczności walidacji z kolejki, wykonuje walidację i informuje API o wyniku walidacji

Wyróżniamy następujące kolejki:

| Nazwa kolejki | Typ | Konsumujący | Opis |
| --- | --- | --- | --- |
| DebtImportQueue_ImportValidation | Rabbit MQ | system DEBT Manager | Kolejka zawierająca importy dla których należy wykonać krok walidacji danych |
| DebtManager.BL.DM.ExternalMessages.ExternalImportValidation, DebtManager.BL.DM.ExternalMessages_ExternalImportValidation | Rabbit MQ | zewnętrzny system | Patrz jak wyżej |
| DebtImportQueue_ImportTransformation | Rabbit MQ | system DEBT Manager | Kolejka zawierająca importy dla których należy wykonać krok transformacji danych z importu na . |
| DebtManager.BL.DM.ExternalMessages.ExternalImportTransformation, DebtManager.BL.DM.ExternalMessages_ExternalImportTransformation | Rabbit MQ | zewnętrzny system | Patrz jak wyżej. |
| DebtImportQueue_ImportFinalization | Rabbit MQ | system DEBT Manager | Kolejka konsumowana przez system DEBT Manager, zawierająca importy które zostały zakończone, dająca możliwość wykonania kolejnych kroków po imporcie np.dla poprawnie zakończonych importów np. uruchomienie workflowów na zaimportowanych sprawachdla niepoprawnie zakończonych importów np. usunięcie danych, jeśli jakieś zostały zaimportowane |
| DebtManager.BL.DM.ExternalMessages.ExternalImportFinalizer, DebtManager.BL.DM.ExternalMessages_ExternalImportFinalizer | Rabbit MQ | zewnętrzny system | Patrz jak wyżej. |
| Baza danych: dm_integration_[nazwa]Schemat: dm_messagesTabela: [nazwa_komunikatu] | tabela SQL Server | system DEBT Manager | Dla każdego z istnieje w bazie API Integracyjnego tabela o nazwie dm_messages.[nazwa_komunikatu], która pełni role kolejki danego komunikatu. Szczegółowy opis zastosowanego rozwiązania znajduje się pod następującym linkiem. |

!!! info "Przykład stanu kolejek"
    Przykład stanu kolejek

    Przykład prezentujący status kolejek API integracyjnego w czasie gdy wykonujemy typ importu obsługiwany przez system zewnętrzny i jesteśmy w trakcie ostatniego korku - finalizacji importu:
