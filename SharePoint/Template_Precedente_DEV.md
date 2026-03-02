# Template_Precedente_DEV — Lista SharePoint

## Descrição
Armazena as precedências entre Tasks e SubTasks de um Template.

## Colunas principais
| Coluna | Tipo | Descrição |
|---|---|---|
| ID | Auto | ID do registro |
| mirror_id | Number | Espelho do próprio ID |
| template_idf | Number | FK → Templates.ID |
| functional_area_idf | Number | FK → AreaFuncional.ID |
| task_idf | Number | ID da Task que TEM a precedência |
| subtask_idf | Number | ID da SubTask que TEM a precedência |
| preceding_task_idf | Number | ID da Task precedente |
| preceding_subtask_idf | Number | ID da SubTask precedente |
| precedence_type | Text | "Task" / "Sub-task" / "Manual Date" |
| StartDate | Date | Data início da precedência |
| EndDate | Date | Data fim da precedência |
| OrdemPrecedente | Number | Ordem da precedência |

## Regras de preenchimento
| precedence_type | preceding_task_idf | preceding_subtask_idf |
|---|---|---|
| Task | ID da Task precedente | Blank |
| Sub-task | Blank | ID da SubTask precedente |
| Manual Date | Blank | Blank |
