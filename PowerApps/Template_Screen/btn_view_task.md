# btn_view_task — View Tasks

Carrega as coleções e abre a tela "Manage Tasks".

## Estratégia
Duas coleções separadas para evitar [object Object]:
- `col_ordertask` — Tasks do Template
- `col_precedentes` — Precedências do Template

## OnSelect
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
);;

ClearCollect(
    col_precedentes;
    ShowColumns(
        Filter(
            Template_Precedente_DEV;
            template_idf = reg_template.ID
        );
        ID;
        template_idf;
        functional_area_idf;
        task_idf;
        preceding_task_idf;
        preceding_subtask_idf;
        precedence_type
    )
);;

Set(varViewTask; true)
```
