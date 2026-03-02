# Template_Task — Lista SharePoint

## Descrição
Armazena as Tasks vinculadas a cada Template.

## Colunas principais
| Coluna | Tipo | Descrição |
|---|---|---|
| ID | Auto | ID do registro |
| mirror_id | Number | Espelho do próprio ID |
| template_idf | Number | FK → Templates.ID |
| TemplateName | Text | Nome do Template |
| functional_area | Text | Nome da Área Funcional |
| functional_area_id | Number | FK → AreaFuncional.ID |
| task_idf | Number | FK → Tasks_processs.ID |
| TaskName | Text | Nome da Task |
| OrdemTask | Number | Ordem no Gantt |
| DurationInDays | Number | Duração em dias |
| StartDate | Date | Data início |
| EndDate | Date | Data fim |
| PrecedentTask | Text | Nome da task precedente (legado) |
| IsMilestone | Boolean | É marco |
| IsVisible | Boolean | Visível no Gantt |
| Status | Choice | Active / Inactive |
