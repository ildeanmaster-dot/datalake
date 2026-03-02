# btn_Precedente — Add Preceding

Salva a precedência de uma Task ou SubTask no Template.

## Bug corrigido
- `preceding_task_idf` estava salvando `vTaskId` (task atual) em vez do ID da task precedente selecionada
- Adicionadas variáveis `vPrecedingTaskId` e `vPrecedingSubTaskId`

## Campos salvos em Template_Precedente_DEV
| Campo | Valor |
|---|---|
| `task_idf` | ID da Task que TEM a precedência |
| `preceding_task_idf` | ID da Task precedente (quando tipo = "Task") |
| `preceding_subtask_idf` | ID da SubTask precedente (quando tipo = "Sub-task") |
| `precedence_type` | "Task" / "Sub-task" / "Manual Date" |

## OnSelect
```powerfx
If(
    !chk_preceding.Checked;
    false;
    If(
        IsBlank(frm_template_cbb_task.Selected.ID);
        Notify("⚠️ Selecione uma Task antes de adicionar um Preceding."; NotificationType.Warning);
        If(
            IsBlank(Preceding_choice.Selected.Value);
            Notify("⚠️ Selecione o tipo de Preceding: Task, Sub-task ou Manual Date."; NotificationType.Warning);
            With(
                {
                    vTemplateId:          reg_template.ID;
                    vAreaId:              frm_template_cbb_functional_area.Selected.ID;
                    vTaskId:              frm_template_cbb_task.Selected.ID;
                    vSubTaskId:           frm_template_cbb_subtask.Selected.ID;
                    vPrecedingType:       Preceding_choice.Selected.Value;
                    vPrecedingTaskId:     frm_template_cbb_peced_task.Selected.ID;
                    vPrecedingSubTaskId:  frm_temp_cbb_peced_subtask.Selected.ID;
                    vNextOrderPrecedente:
                        IfError(
                            Max(
                                Filter(
                                    Template_Precedente_DEV;
                                    template_idf        = reg_template.ID &&
                                    functional_area_idf = frm_template_cbb_functional_area.Selected.ID &&
                                    task_idf            = frm_template_cbb_task.Selected.ID
                                );
                                OrdemPrecedente
                            );
                            0
                        ) + 1
                };
                With(
                    {
                        rExisting: LookUp(
                            Template_Precedente_DEV;
                            template_idf        = vTemplateId  &&
                            functional_area_idf = vAreaId      &&
                            task_idf            = vTaskId      &&
                            precedence_type     = vPrecedingType
                        )
                    };
                    If(
                        !IsBlank(rExisting.ID);
                        Notify("ℹ️ Preceding já registrado para esta Task."; NotificationType.Information);
                        With(
                            {
                                rResult: IfError(
                                    Patch(
                                        Template_Precedente_DEV;
                                        Defaults(Template_Precedente_DEV);
                                        {
                                            template_idf:          vTemplateId;
                                            functional_area_idf:   vAreaId;
                                            task_idf:              vTaskId;
                                            subtask_idf:           If(vPrecedingType = "Sub-task"; vSubTaskId; Blank());
                                            preceding_task_idf:    If(vPrecedingType = "Task"; vPrecedingTaskId; Blank());
                                            preceding_subtask_idf: If(vPrecedingType = "Sub-task"; vPrecedingSubTaskId; Blank());
                                            StartDate:             frm_template_sd_start_preceding.SelectedDate;
                                            EndDate:               frm_template_sd_end_preceding.SelectedDate;
                                            precedence_type:       vPrecedingType;
                                            OrdemPrecedente:       vNextOrderPrecedente
                                        }
                                    );
                                    Blank()
                                )
                            };
                            If(
                                IsBlank(rResult.ID);
                                Notify("❌ Erro ao salvar Preceding."; NotificationType.Error);
                                With(
                                    {
                                        rMirror: IfError(
                                            Patch(Template_Precedente_DEV; rResult; { mirror_id: rResult.ID });
                                            Blank()
                                        )
                                    };
                                    If(
                                        IsBlank(rMirror.ID);
                                        Notify("⚠️ Preceding salvo sem índice espelho. ID: " & rResult.ID; NotificationType.Warning);
                                        Notify("✅ Preceding adicionado! Tipo: " & vPrecedingType & " · Ordem " & vNextOrderPrecedente; NotificationType.Success);;
                                        Reset(frm_template_cbb_peced_task);;
                                        Reset(frm_template_sd_end_preceding);;
                                        Reset(frm_temp_cbb_peced_subtask);;
                                        Reset(frm_template_sd_start_preceding);;
                                        Reset(Preceding_choice)
                                    )
                                )
                            )
                        )
                    )
                )
            )
        )
    )
)
```
