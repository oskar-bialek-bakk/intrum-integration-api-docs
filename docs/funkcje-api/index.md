---
title: "Funkcje API"
---

Niniejszy rozdział zawiera opis funkcji API wykorzystywanych do przeprowadzania importów/eksportów danych dla systemu DEBT Manager.

## Importy

Grupa endpointów odpowiedzialna za cały cykl życia importu danych — od przesłania pliku, przez przetwarzanie komunikatów, po monitorowanie statusu i pobieranie wyników.

| Endpoint | Metoda | Opis |
|----------|--------|------|
| [ImportFromFileUpload](importy/import-from-file-upload.md) | POST | Import danych z pliku przesłanego przez HTTP |
| [ImportFromFileShare](importy/import-from-file-share.md) | POST | Import danych z pliku na udziale sieciowym |
| [EnqueImportMessage](importy/enque-import-message.md) | POST | Zakolejkowanie pojedynczego komunikatu importowego |
| [GetImportStatus](importy/get-import-status.md) | GET | Pobranie statusu importu |
| [GetObjectStatusByID](importy/get-object-status-by-id.md) | GET | Pobranie statusu obiektu po identyfikatorze |
| [GetImportFile](importy/get-import-file.md) | GET | Pobranie pliku importowego (HTTP) |
| [GetImportFileFromFileShare](importy/get-import-file-from-file-share.md) | GET | Pobranie pliku importowego z udziału sieciowego |
| [SetImportStep](importy/set-import-step.md) | POST | Ustawienie etapu przetwarzania importu |
| [SetImportValidations](importy/set-import-validations.md) | POST | Konfiguracja walidacji dla importu |
| [GetDictionaries](importy/get-dictionaries.md) | GET | Pobranie słowników walidacyjnych |
| [RefreshDictionaries](importy/refresh-dictionaries.md) | GET | Odświeżenie cache słowników |
| [GetMailRecipients](importy/get-mail-recipients.md) | GET | Pobranie listy odbiorców e-mail |
| [GetMailRecipientsForImportType](importy/get-mail-recipients-for-import-type.md) | GET | Pobranie odbiorców e-mail dla typu importu |
