# 📐 Arquitetura — Template Management EIS Projects

## Visão Geral

```
Power Apps (Frontend)
    └── SharePoint (Backend)
          ├── Templates
          ├── Template_Task
          ├── Template_SubTask
          ├── Template_Precedente_DEV
          ├── Tasks_processs
          ├── Sub_task
          ├── AreaFuncional
          └── BusinessUnit
```

## Fluxo de dados

```
1. Usuário seleciona Template
   └── reg_template = LookUp(Templates, ID = vId)

2. Cadastro de Task
   └── btn_Task → Patch(Template_Task)
         ├── DurationInDays ← Tasks_processs.DurationInDays
         ├── StartDate ← DatePicker
         └── EndDate   ← DatePicker

3. Cadastro de Preceding
   └── btn_Precedente → Patch(Template_Precedente_DEV)
         ├── task_idf              = Task atual
         ├── preceding_task_idf    = Task precedente selecionada
         └── preceding_subtask_idf = SubTask precedente selecionada

4. Visualização
   └── btn_view_task
         ├── col_ordertask    ← Template_Task
         └── col_precedentes  ← Template_Precedente_DEV

5. Gallery "Manage Tasks"
   └── Label Start condition
         ├── col_precedentes vazio → "Independent Task"
         ├── precedence_type = "Task"    → Tasks_processs.Name
         └── precedence_type = "Sub-task" → Sub_task.name
```

## Coleções locais

| Coleção | Fonte | Uso |
|---|---|---|
| `col_ordertask` | Template_Task | Gallery Manage Tasks |
| `col_precedentes` | Template_Precedente_DEV | Label Start condition |
| `colTasksNoTemplate` | Template_Task | DatePicker End Date Preceding |
| `colSubTasksNoTemplate` | Template_SubTask | DatePicker End Date Preceding |
