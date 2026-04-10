---
title: "How-to: Dodanie nowego typu importu obsługiwanego zewnętrznie"
---

W celu dodania nowego typu importu obsługiwanego zewnętrznie należy wykonać poniższy skrypt SQL na bazie integration uzupełniając wcześniej w sekcji DECLARE odpowiednie wartości zmiennych:

```sql
DECLARE
@dm_import_type_id int = 200-- unikalne ID typu importu w bazie DEBT Manager (import_typ.impt_id)
,@import_type_name varchar(500) = 'My New case import' -- nazwa typu importu
,@integartion_import_type_id int = 200 -- nazwa importu w bazie integacyjnej API
,@dm_orginal_creditor_id int = 10013 -- ID kontrahenta pod którego importujemy sprawy (kontrahent.ko_id) - jeśli nowy kontrahent należy go dodac do tabeli
,@dm_portfolio_id int = 94 -- ID portfela pod który importujemy sprawy (umowa_kontrahent.uko_id) - jeśli nowy portfel należy go dodać do tabeli


INSERT INTO [dbo].[import_typ] ([impt_id], [impt_nazwa], [impt_class], [impt_isAsynchronous], [impt_is_webapi], [impt_uuid])
select @dm_import_type_id, @import_type_name, 'DebtManager.BL.DataIntegration.Core.Import.IntegrationsApiImport', 1, 1, newid()


INSERT INTO [dm_config].[import_type]([id], [name], [validation_class_name], [transformation_class_name], [finalization_class_name], [dm_impt_id], [dm_ko_id], [dm_uko_id])
select @integartion_import_type_id, @import_type_name,
'DebtManager.BL.IntegrationsAPI.Core.ExternalRabbitMqValidator','DebtManager.BL.IntegrationsAPI.Core.ExternalRabbitMqTransformator','DebtManager.BL.IntegrationsAPI.Core.ExternalRabbitMqFinalizer',
@dm_import_type_id, @dm_orginal_creditor_id,@dm_portfolio_id







```
