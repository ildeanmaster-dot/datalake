# col_ordertask — Tasks do Template

## Descrição
Coleção local com as Tasks do Template selecionado.
Carregada no `btn_view_task` OnSelect.

## Fórmula
```powerfx
ClearCollect(
    col_ordertask;
    ShowColumns(
        Filter(
            Template_Task;
            template_idf = reg_template.ID
        );
        ID;
        task_idf;
        TaskName;
        functional_area;
        OrdemTask;
        PrecedentTask;
        StartDate;
        EndDate;
        IsMilestone;
        IsVisible;
        Status
    )
)
```

## Colunas
| Coluna | Tipo | Descrição |
|---|---|---|
| ID | Number | ID do registro em Template_Task |
| task_idf | Number | ID da Task no catálogo Tasks_processs |
| TaskName | Text | Nome da Task |
| functional_area | Text | Nome da Área Funcional |
| OrdemTask | Number | Ordem de exibição no Gantt |
| PrecedentTask | Text | Nome da task precedente (legado) |
| StartDate | Date | Data início da Task |
| EndDate | Date | Data fim da Task |
| IsMilestone | Boolean | É marco do projeto |
| IsVisible | Boolean | Visível no Gantt Chart |
| Status | Choice | Active / Inactive |
