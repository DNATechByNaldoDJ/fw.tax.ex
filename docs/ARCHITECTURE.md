# 🏛 FWTaxEx – Architecture

## 📦 Core Classes

### FWTaxExEngine
Main entry point of the tax engine. Orchestrates rules, calculations, and logging.

- `New()`  
- `LoadRules(cSQLiteFile)`  
- `CalcFromJSON(cJSON)`  
- `SetLogger(oLogger)`

### FWTaxExRule
Represents a single tax rule.

- Attributes: `tax_name`, `ncm_code`, `product_type`, `base_formula`, `rate`, `fixed_value`, `legal_ref`, `conditions`  
- Methods: `Apply(oItem) -> FWTaxExCalc`

### FWTaxExCalc
Holds a single tax calculation.

- Attributes: `tax_name`, `base`, `rate`, `value`  
- Methods: `ToJSON()`

### FWTaxExResult
Aggregates all calculations for an invoice.

- Attributes: `invoice_number`, `aCalcs`  
- Methods: `AddCalc(oCalc)`, `ToJSON()`

### FWTaxExLog
Persists audit logs.

- Attributes: `cDBFile`  
- Methods: `Log(cInvoice, cMessage)`, `GetLogs(cInvoice)`

---

## 🔀 Mappers Layer

### FWTaxExMapper
Standardizes invoice data from different ERPs into a single JSON format.

- Methods:  
  - `MapFromJSON(cERP, cJSON)`  
  - `MapFromProtheus(cJSON)`  
  - `MapFromDatasul(cJSON)`  
  - `MapFromSAP(cJSON)`  
  - `MapFromGeneric(cJSON)`

**Workflow:** ERP JSON → Mapper → Standard JSON → Engine

---

## 🔗 Relationships (Diagram)

```
+----------------+
| FWTaxExEngine  |
+----------------+
       |
       | uses
       v
+----------------+       +------------------+
| FWTaxExMapper  |-----> | FWTaxExRule      |
+----------------+       +------------------+
       |                         |
       | standardizes             | applies to
       v                         v
+----------------+       +------------------+
| Standard JSON  |-----> | FWTaxExCalc      |
+----------------+       +------------------+
                               |
                               | aggregated into
                               v
                        +----------------+
                        | FWTaxExResult  |
                        +----------------+
                               |
                               | logged by
                               v
                        +----------------+
                        | FWTaxExLog     | ---> SQLite (logs)
                        +----------------+
```

---

## Benefits of Mappers Layer

- ERP Flexibility → any ERP only needs its specific adapter.  
- Engine Isolation → engine logic doesn’t break if ERP structure changes.  
- Community Contribution → adding new ERP adapters is simple.  
- Standardization → consistent JSON structure for all calculations.
