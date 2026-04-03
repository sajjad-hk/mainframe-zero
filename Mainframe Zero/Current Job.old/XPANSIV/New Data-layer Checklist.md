# APX/Graphite

## Overview
This checklist covers creating a new feature module in the APX/Graphite framework with the standard layered architecture: Database → Domain → Business → DAO → Spring Configuration.

---

## Phase 1: Database Layer

### Tables
- [ ] Create main entity table with standard columns:
  - Primary key column (e.g., `[PREFIX]_[ENTITY]_ID`)
  - Foreign key columns to related entities
  - `CREATED_DT` (DATETIME2, NOT NULL, DEFAULT sysdatetime())
  - `CREATED_BY` (BIGINT, NOT NULL)
  - `LAST_UPDATED_DT` (DATETIME2, NULL)
  - `LAST_UPDATED_BY` (BIGINT, NULL)
  - `UCON_USER_CONTEXT_ID` (BIGINT, NULL, FK to USER_CONTEXT_UCON)
  - `VERSION_NR` (BIGINT, DEFAULT 0, NOT NULL) — for optimistic locking

- [ ] Create reference data table(s) if needed (status types, etc.):
  - `[PREFIX]_STATUS_TYPE_ID` (PK)
  - `STATUS_TYPE_CD` (unique)
  - `STATUS_TYPE_NM`
  - `STATUS_TYPE_DESC`
  - Standard audit columns

- [ ] Create indexes on foreign key columns

### Sequences
- [ ] Create sequence for main entity: `[ENTITY]_ID_SQ`

### Reference Data
- [ ] Create MERGE script to populate reference data values

---

## Phase 2: Reference Data Classes

For each reference data type:

- [ ] Create class extending `BaseReferenceImpl`
- [ ] Add `@Enumerated(type = ReferenceDataType.[TYPE_NAME])` annotation
- [ ] Define static constants for each code value
- [ ] Add private constructor with `String code` parameter
- [ ] Add public constructor with `IDReferenceDataObject wrapped` parameter
- [ ] Add static `getByCode(String code)` method
- [ ] **Add entry to `ReferenceDataType` enum** (manual step)

**Package:** `com.apx.[module_name].referencedata`

**Template:**
```java
@Enumerated(type = ReferenceDataType.[TYPE_NAME])
public class [TypeName] extends BaseReferenceImpl {

    public static final [TypeName] CODE1 = new [TypeName]("CODE1");
    public static final [TypeName] CODE2 = new [TypeName]("CODE2");

    private [TypeName](String code) {
        super(code);
    }

    public [TypeName](IDReferenceDataObject wrapped) {
        super(wrapped);
    }

    public static [TypeName] getByCode(String code) {
        return getByCode([TypeName].class, code);
    }
}
```

---

## Phase 3: Domain Layer

### Domain Interface (ID[EntityName])
- [ ] Create interface extending `IDomainObject`
- [ ] Define getter/setter for each business field (FK IDs as `Long`)
- [ ] Do NOT include audit fields (inherited from base)

**Package:** `com.apx.[module_name].datalayer.data.domain.api`

### Domain Implementation (D[EntityName]Impl)
- [ ] Create class extending `AbstractDomainObject`
- [ ] Implement domain interface
- [ ] Define private fields for each property
- [ ] Implement getters/setters
- [ ] Override `marshal(IGraphiteMarshaller)` method
- [ ] Override `unmarshal(IGraphiteUnmarshaller)` method
- [ ] **Marshal/unmarshal order must match field declaration order**

**Package:** `com.apx.[module_name].datalayer.data.domain.impl`

---

## Phase 4: Business Layer

### Business Interface (IB[EntityName])
- [ ] Create interface extending `IBusinessObject`
- [ ] Define getter/setter for each field ID (`Long`)
- [ ] Define getter/setter for typed reference objects (e.g., `StatusType getStatusType()`)
- [ ] Add audit field accessors: `getCreatedDt()`, `getCreatedBy()`, `setCreatedBy()`

**Package:** `com.apx.[module_name].datalayer.data.business.api`

### Business Implementation (B[EntityName]Impl)
- [ ] Create class extending `AbstractBusinessObject<ID[EntityName]>`
- [ ] Implement business interface
- [ ] Constructor creates new domain impl: `super(new D[EntityName]Impl())`
- [ ] Delegate to `getDomainObject()` for all getters
- [ ] Call `setState(State.MODIFIED)` before all setters
- [ ] Use `getReference([Type].class, id)` for typed reference getters
- [ ] Handle null in reference setters: `ref != null ? ref.getId() : null`

**Package:** `com.apx.[module_name].datalayer.data.business.impl`

### Factory ([EntityName]Factory)
- [ ] Create class extending `BaseBusinessObjectFactory`
- [ ] Define `SPRING_ROLE` constant matching Spring bean name
- [ ] Add static `getInstance()` method using `SpringAccess.get(SPRING_ROLE)`
- [ ] Add `create[EntityName]()` method returning business interface

**Package:** `com.apx.[module_name].datalayer.data.business.api`

---

## Phase 5: DAO Layer

### DAO Interface (IB[EntityName]DAO)
- [ ] Create interface extending `IBusinessDAO<IB[EntityName]>`
- [ ] Add any custom query methods

**Package:** `com.apx.[module_name].datalayer.service.api`

### DAO Implementation (B[EntityName]DAOImpl)
- [ ] Create class extending `BaseBusinessDAOImpl<IB[EntityName]>`
- [ ] Implement DAO interface
- [ ] Implement custom query methods using `getHibernateHelper().executeNamedQueryForSingleObject()`

**Package:** `com.apx.[module_name].datalayer.service.impl`

---

## Phase 6: Hibernate Mapping

### HBM File ([EntityName]_hbm.xml)
- [ ] Define `<class>` with full impl class name, table name, schema
- [ ] Define `<id>` with column name
- [ ] Define `<version>` for optimistic locking (`VERSION_NR`)
- [ ] Define `<property>` for each field:
  - Use `type="java.lang.Long"` for nullable Long fields
  - Use `type="long"` for required long fields
  - Use `type="string"` with `length` for varchar
  - Use `type="text"` for NVARCHAR(MAX)/CLOB
  - Use `type="java.util.Date"` for datetime
  - Add `not-null="true"` for required columns
- [ ] Define `<sql-query>` for any named queries

### Sequence Queries (sequences-queries_hbm.xml)
- [ ] **Add CURRENT_VALUE query** (manual step)
- [ ] **Add NEXT_VALUE query** (manual step)
- [ ] **Add INCREMENT query** (manual step)

**Template:**
```xml
<sql-query name="[SEQUENCE_NAME].CURRENT_VALUE">
    <![CDATA[select cast(current_value as bigint) as CURRENT_VALUE_LONG from sys.sequences WHERE name='[SEQUENCE_NAME]']]>
    <return-scalar column="CURRENT_VALUE_LONG" type="java.lang.Long"/>
</sql-query>

<sql-query name="[SEQUENCE_NAME].NEXT_VALUE">
    <![CDATA[select next value for [SEQUENCE_NAME] NEXT_VALUE]]>
    <return-scalar column="NEXT_VALUE" type="java.lang.Long"/>
</sql-query>

<sql-query name="[SEQUENCE_NAME].INCREMENT">
    <![CDATA[select cast(increment as bigint) as INCREMENT from sys.sequences WHERE name='[SEQUENCE_NAME]']]>
    <return-scalar column="INCREMENT" type="java.lang.Long"/>
</sql-query>
```

---

## Phase 7: Spring Configuration

### Module XML (carbon-[module]-module.xml)
- [ ] Import DAO XML
- [ ] Import services XML

### DAO XML (carbon-[module]-dao.xml)
- [ ] Define Factory bean with:
  - `idGeneratorFactory` ref
  - `interfaceToSupplier` map
- [ ] Define base DAO bean (abstract, parent of `baseBusinessDAObean`)
- [ ] Define Supplier bean (`ReflectionBusinessObjectSupplier`)
- [ ] Define Domain DAO bean with:
  - `cacheName`
  - `domainInterfaceType`
  - `persistentClass`

### Services XML (carbon-[module]-services.xml)
- [ ] Define Business DAO bean with:
  - `businessObjectInterfaceType`
  - `correspondingDomainDAO` ref

### Main Config Updates
- [ ] **Add module to `smartConfigSpringLocations`** (manual step)
- [ ] **Add cache name to cache configuration** (manual step)

---

## Final Verification Checklist

- [ ] All package names follow convention: `com.apx.[module_name].[layer].[sublayer].[api|impl]`
- [ ] All class names follow convention: `ID*`, `D*Impl`, `IB*`, `B*Impl`, `*Factory`, `IB*DAO`, `B*DAOImpl`
- [ ] Marshal/unmarshal field order matches
- [ ] HBM property names match domain impl field names exactly
- [ ] Spring bean names are consistent across configs
- [ ] Factory `SPRING_ROLE` matches Spring bean name
- [ ] All FK columns have corresponding indexes in database

---

## Quick Reference: Layer Dependencies

```
┌─────────────────────────────────────────────────────────────┐
│                     Spring Configuration                    │
│  (wires everything together)                                │
├─────────────────────────────────────────────────────────────┤
│                         DAO Layer                           │
│  IB*DAO ← B*DAOImpl (extends BaseBusinessDAOImpl)           │
├─────────────────────────────────────────────────────────────┤
│                      Business Layer                         │
│  IB* ← B*Impl (extends AbstractBusinessObject<ID*>)         │
│  *Factory (extends BaseBusinessObjectFactory)               │
├─────────────────────────────────────────────────────────────┤
│                       Domain Layer                          │
│  ID* ← D*Impl (extends AbstractDomainObject)                │
├─────────────────────────────────────────────────────────────┤
│                    Reference Data Layer                     │
│  *Type (extends BaseReferenceImpl)                          │
├─────────────────────────────────────────────────────────────┤
│                     Hibernate Mapping                       │
│  *_hbm.xml (maps D*Impl to database table)                  │
├─────────────────────────────────────────────────────────────┤
│                      Database Layer                         │
│  Tables, Sequences, Reference Data                          │
└─────────────────────────────────────────────────────────────┘
```

---

## Common Pitfalls

1. **Wrong DAO wired in Spring** — Double-check `correspondingDomainDAO` ref matches the correct Domain DAO bean name
2. **Marshal/unmarshal order mismatch** — Fields must be in exact same order
3. **Missing `setState(State.MODIFIED)`** — All setters in Business impl must call this
4. **Nullable vs primitive types** — Use `Long` for nullable, `long` for required in HBM
5. **Cache name not registered** — Add to cache config or you'll get runtime errors
6. **Sequence not in sequences-queries_hbm.xml** — ID generation will fail
7. **ReferenceDataType enum not updated** — Reference data won't load
