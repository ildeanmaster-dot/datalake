# 📦 Power Apps — Template Management EIS Projects

Documentação técnica do sistema de gestão de Templates EIS Projects.
Desenvolvido em **Power Apps + SharePoint**.

## 🏗️ Arquitetura
```
Templates
  └── Template_Task
        └── Template_SubTask
              └── Template_Precedente_DEV
```

## 📋 Listas SharePoint
| Lista | Descrição |
|---|---|
| `Templates` | Cabeçalho (Name, Status, StartDate, EndDate, BusinessUnit) |
| `Template_Task` | Tasks vinculadas ao template |
| `Template_SubTask` | SubTasks vinculadas às Tasks |
| `Template_Precedente_DEV` | Precedências entre Tasks/SubTasks |
| `Tasks_processs` | Catálogo de Tasks (DurationInDays) |
| `Sub_task` | Catálogo de SubTasks (durationindays) |
| `AreaFuncional` | Áreas funcionais por Business Unit |
| `BusinessUnit` | Unidades de negócio |

## 🔄 Fluxo de Cadastro
```
1. Functional Area → Task → SubTask (opcional) → Preceding (opcional)
2. btn_Task       → Template_Task      (StartDate, EndDate, DurationInDays)
3. btn_SubTask    → Template_SubTask
4. btn_Precedente → Template_Precedente_DEV
                    preceding_task_idf    = ID Task precedente
                    preceding_subtask_idf = ID SubTask precedente
5. btn_view_task  → col_ordertask + col_precedentes → Manage Tasks
```

## 📁 Estrutura
```
datalake/
├── README.md
├── PowerApps/
│   ├── Template_Screen/
│   │   ├── btn_Task.md
│   │   ├── btn_SubTask.md
│   │   ├── btn_Precedente.md
│   │   └── btn_view_task.md
│   └── Collections/
│       ├── col_ordertask.md
│       └── col_precedentes.md
├── SharePoint/
│   ├── Template_Task.md
│   ├── Template_SubTask.md
│   └── Template_Precedente_DEV.md
└── docs/
    └── arquitetura.md
```

## 👨‍💻 Desenvolvido por
Freitas — Data Engineering & Power Platform
