# col_precedentes — Precedências do Template

## Descrição
Coleção local com as precedências do Template selecionado.
Carregada no `btn_view_task` OnSelect.

## Fórmula
```powerfx
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
)
```

## Colunas
| Coluna | Tipo | Descrição |
|---|---|---|
| ID | Number | ID do registro |
| template_idf | Number | ID do Template |
| functional_area_idf | Number | ID da Área Funcional |
| task_idf | Number | ID da Task que TEM precedente |
| preceding_task_idf | Number | ID da Task precedente |
| preceding_subtask_idf | Number | ID da SubTask precedente |
| precedence_type | Text | "Task" / "Sub-task" / "Manual Date" |

## Uso — Label "Start condition" na Gallery
```powerfx
With(
    {
        vPrec: LookUp(
            col_precedentes;
            template_idf = reg_template.ID &&
            task_idf     = ThisItem.task_idf
        )
    };
    If(
        IsBlank(vPrec.ID);
        "Independent Task";
        If(
            vPrec.precedence_type = "Task";
            LookUp(Tasks_processs; ID = vPrec.preceding_task_idf).Name;
            LookUp(Sub_task; ID = vPrec.preceding_subtask_idf).name
        )
    )
)
```
