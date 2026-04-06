# Database Table Creation Checklist

## 1. DDL Scripts (Tables, Sequences, Views)

- [ ] **Create table script**: `EM_OWNER\Tables\<TABLE_NAME>.sql`
- [ ] **Create sequence script** (if not using IDENTITY): `EM_OWNER\Sequences\<SEQUENCE_NAME>.sql`
- [ ] **Create reference data tables** (if needed): `EM_OWNER\Tables\<REF_DATA_TABLE>.sql`

---

## 2. Reference Data Seed Scripts

- [ ] **Create seed data script**: `EM_OWNER\Reference-Data\EM_OWNER.<REF_DATA_TABLE>.sql`
  - Use MERGE pattern with `DatabaseScriptLog_AddScriptExecution`
  - Set `@version = 1` for new scripts

---

## 3. Project File Registration (`XPCProd.sqlproj`)

- [ ] **Add table(s)** to `<Build Include="..."/>`:

```xml
<Build Include="EM_OWNER\Tables\<TABLE_NAME>.sql" />
```

- [ ] **Add sequence(s)** (if applicable):

```xml
<Build Include="EM_OWNER\Sequences\<SEQUENCE_NAME>.sql" />
```

- [ ] **Add reference data tables**:

```xml
<Build Include="EM_OWNER\Tables\<REF_DATA_TABLE>.sql" />
```

---

## 4. Post-Deployment Script (`Script.PostDeployment.sql`)

- [ ] **Add reference data execution**:

```sql
:r ".\EM_OWNER\Reference-Data\EM_OWNER.<REF_DATA_TABLE>.sql"
GO
```

---

## 5. Reference Data View (`PT_REF_DATA_MV`)

- [ ] **Add UNION statement** for each new reference data table:

```sql
UNION
    SELECT '<TABLE_NAME>' AS TABLE_NM,
           <PK_COLUMN> AS ID,
           <CODE_COLUMN> AS CODE,
           <NAME_COLUMN> AS NAME,
           <DESC_COLUMN> AS DES,
           NULL AS EFFECTIVE_DT,
           NULL AS END_DT,
           CREATED_DT,
           CREATED_BY,
           LAST_UPDATED_DT,
           LAST_UPDATED_BY,
           NULL AS DISPLAY_SEQ
    FROM EM_OWNER.<TABLE_NAME>
```

---

## 6. Standard Columns (Required by AbstractDomainObject)

| Column                 | Type         | Nullable | Default           | Notes                     |
| ---------------------- | ------------ | -------- | ----------------- | ------------------------- |
| `CREATED_DT`           | DATETIME2(0) | NOT NULL | `SYSDATETIME()`   |                           |
| `CREATED_BY`           | BIGINT       | NOT NULL |                   |                           |
| `LAST_UPDATED_DT`      | DATETIME2(0) | NULL     |                   |                           |
| `LAST_UPDATED_BY`      | BIGINT       | NULL     |                   |                           |
| `UCON_USER_CONTEXT_ID` | BIGINT       | NULL     |                   | FK to `USER_CONTEXT_UCON` |
| `VERSION_NR`           | BIGINT       | NOT NULL | `0`               | Optimistic locking        |

---

## 7. Naming Conventions

| Element       | Pattern                                    | Example                                |
| ------------- | ------------------------------------------ | -------------------------------------- |
| Table name    | `<DESCRIPTIVE_NAME>_<ABBREV>`              | `REGISTRY_ACCESS_FAILURE_TASK_RAFT`    |
| PK column     | `<ABBREV>_<DESCRIPTIVE_NAME>_ID`           | `RAFT_REGISTRY_ACCESS_FAILURE_TASK_ID` |
| FK columns    | `<REFERENCED_ABBREV>_<REFERENCED_PK_NAME>` | `REGI_REGISTRY_PROGRAM_ID`             |
| Sequence      | `<TABLE_NAME>_ID_SQ`                       | `REGISTRY_ACCESS_FAILURE_TASK_ID_SQ`   |
| PK Constraint | `<ABBREV>_PK`                              | `RAFT_PK`                              |
| FK Constraint | `<ABBREV>_<REF_ABBREV>_FK`                 | `RAFT_REGI_FK`                         |
| Index         | `IX_<ABBREV>_<COLUMN_NAME>`                | `IX_RAFT_STATUS_TYPE`                  |

---

## 8. Template: Reference Data Seed Script

```sql
/****************************************************************************
  Reference data for EMA

  tables affected:
        EM_OWNER.<TABLE_NAME>
****************************************************************************/
BEGIN TRY
    BEGIN TRAN;

    DECLARE @executeScriptNow bit;
    EXEC dbo.DatabaseScriptLog_AddScriptExecution
         @script_name = 'EM_OWNER.<TABLE_NAME>.sql'
        , @version = 1
        , @execute_now = @executeScriptNow OUTPUT;

    IF @executeScriptNow = 1
        BEGIN
            MERGE INTO [EM_OWNER].[<TABLE_NAME>] AS [Target]
            USING (VALUES 
                (1, 'CODE1', 'Name 1', 'Description 1', '2026-03-26 00:00:00', 100, NULL, NULL, NULL, 0),
                (2, 'CODE2', 'Name 2', 'Description 2', '2026-03-26 00:00:00', 100, NULL, NULL, NULL, 0)
            ) AS [Source] (
                [<PK_COLUMN>],
                [<CODE_COLUMN>],
                [<NAME_COLUMN>],
                [<DESC_COLUMN>],
                [CREATED_DT],
                [CREATED_BY],
                [LAST_UPDATED_DT],
                [LAST_UPDATED_BY],
                [UCON_USER_CONTEXT_ID],
                [VERSION_NR]
            )
            ON ([Target].[<PK_COLUMN>] = [Source].[<PK_COLUMN>])
            WHEN MATCHED AND (
                NULLIF([Source].[<CODE_COLUMN>], [Target].[<CODE_COLUMN>]) IS NOT NULL OR
                NULLIF([Source].[<NAME_COLUMN>], [Target].[<NAME_COLUMN>]) IS NOT NULL OR
                NULLIF([Source].[<DESC_COLUMN>], [Target].[<DESC_COLUMN>]) IS NOT NULL
            )
            THEN UPDATE SET 
                [Target].[<CODE_COLUMN>] = [Source].[<CODE_COLUMN>],
                [Target].[<NAME_COLUMN>] = [Source].[<NAME_COLUMN>],
                [Target].[<DESC_COLUMN>] = [Source].[<DESC_COLUMN>],
                [Target].[LAST_UPDATED_DT] = SYSDATETIME(),
                [Target].[LAST_UPDATED_BY] = 100
            WHEN NOT MATCHED BY TARGET
            THEN INSERT (
                [<PK_COLUMN>], [<CODE_COLUMN>], [<NAME_COLUMN>], [<DESC_COLUMN>],
                [CREATED_DT], [CREATED_BY], [LAST_UPDATED_DT], [LAST_UPDATED_BY],
                [UCON_USER_CONTEXT_ID], [VERSION_NR]
            )
            VALUES (
                [Source].[<PK_COLUMN>], [Source].[<CODE_COLUMN>], [Source].[<NAME_COLUMN>], [Source].[<DESC_COLUMN>],
                [Source].[CREATED_DT], [Source].[CREATED_BY], [Source].[LAST_UPDATED_DT], [Source].[LAST_UPDATED_BY],
                [Source].[UCON_USER_CONTEXT_ID], [Source].[VERSION_NR]
            );

            PRINT '[EM_OWNER].[<TABLE_NAME>] rows affected by MERGE: ' + CAST(@@ROWCOUNT AS VARCHAR(100));
        END;

    COMMIT TRAN;
END TRY
BEGIN CATCH
    DECLARE @ErrorMessage nvarchar(4000), @ErrorSeverity int, @ErrorState int;
    SELECT @ErrorMessage = ERROR_MESSAGE(), @ErrorSeverity = ERROR_SEVERITY(), @ErrorState = ERROR_STATE();
    ROLLBACK TRAN;
    RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
END CATCH;
GO
```

---

## 9. Quick Reference: File Locations

| File Type              | Location                                          |
| ---------------------- | ------------------------------------------------- |
| Table DDL              | `EM_OWNER\Tables\`                                |
| Sequence DDL           | `EM_OWNER\Sequences\`                             |
| Reference Data Seed    | `EM_OWNER\Reference-Data\`                        |
| Project File           | `XPCProd.sqlproj`                                 |
| Post-Deployment Script | `Script.PostDeployment.sql`                       |
| Reference Data View    | `EM_OWNER\Views\PT_REF_DATA_MV.sql`               |

---

## Related Links

- [[Java Domain Object Checklist]]
- [[Hibernate Mapping Checklist]]
- [[DAO Implementation Checklist]]
