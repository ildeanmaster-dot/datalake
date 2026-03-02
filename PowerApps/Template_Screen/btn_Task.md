# btn_Task — Add Task

Salva uma Task no Template, incluindo DurationInDays, StartDate e EndDate.

## Correções aplicadas
- Adicionado `DurationInDays`, `StartDate`, `EndDate` no Patch
- `ClearCollect(colTasksNoTemplate)` após salvar para atualizar coleção local

## OnSelect
```powerfx
With(
    {
        vTemplateId:    reg_template.ID;
        vTemplateName:  reg_template.Name;
        vArea:          frm_template_cbb_functional_area.Selected.NameArea;
        vAreaId:        frm_template_cbb_functional_area.Selected.ID;
        vTaskId:        frm_template_cbb_task.Selected.ID;
        vTaskName:      Text(frm_template_cbb_task.Selected.Name);
        vDuration:      frm_template_cbb_task.Selected.DurationInDays;
        vStartDate:     frm_temp_sd_start_task.SelectedDate;
        vEndDate:       frm_temp_sd_end__task.SelectedDate;
        vNextOrder:     IfError(
                            Max(
                                Filter(Template_Task; template_idf = reg_template.ID);
                                OrdemTask
                            );
                            0
                        ) + 1
    };
    If(
        IsBlank(vTemplateId) || IsBlank(vAreaId) || IsBlank(vTaskId);
        Notify("⚠️ Template, Área Funcional e Task são obrigatórios."; NotificationType.Warning);
        With(
            {
                rExisting: LookUp(
                    Template_Task;
                    template_idf       = vTemplateId &&
                    functional_area_id = vAreaId     &&
                    task_idf           = vTaskId
                )
            };
            If(
                !IsBlank(rExisting.ID);
                Notify("ℹ️ Task já vinculada ao Template."; NotificationType.Information);
                With(
                    {
                        rResult: IfError(
                            Patch(
                                Template_Task;
                                Defaults(Template_Task);
                                {
                                    template_idf:       vTemplateId;
                                    TemplateName:       vTemplateName;
                                    functional_area:    vArea;
                                    functional_area_id: vAreaId;
                                    task_idf:           vTaskId;
                                    TaskName:           vTaskName;
                                    OrdemTask:          vNextOrder;
                                    DurationInDays:     vDuration;
                                    StartDate:          vStartDate;
                                    EndDate:            vEndDate;
                                    Status:             { Value: "Active" }
                                }
                            );
                            Blank()
                        )
                    };
                    If(
                        IsBlank(rResult.ID);
                        Notify("❌ Erro ao salvar Task."; NotificationType.Error);
                        With(
                            {
                                rMirror: IfError(
                                    Patch(Template_Task; rResult; { mirror_id: rResult.ID });
                                    Blank()
                                )
                            };
                            If(
                                IsBlank(rMirror.ID);
                                Notify("⚠️ Task salva sem índice espelho. ID: " & rResult.ID; NotificationType.Warning);
                                Notify("✅ Task adicionada: " & vTaskName & " · Ordem " & vNextOrder; NotificationType.Success);;
                                ClearCollect(
                                    colTasksNoTemplate;
                                    Filter(
                                        Template_Task;
                                        template_idf       = reg_template.ID &&
                                        functional_area_id = frm_template_cbb_functional_area.Selected.ID
                                    )
                                );;
                                Reset(frm_template_cbb_task);;
                                Reset(frm_temp_sd_start_task);;
                                Reset(frm_temp_sd_end__task)
                            )
                        )
                    )
                )
            )
        )
    )
)
```
