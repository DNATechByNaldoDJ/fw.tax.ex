# 🗄 FWTaxEx – Database Schema

## 1. Rules Database (FWTaxEx_vYYYY_MM.db)

### metadata
- id (PK)
- version
- effective_from
- effective_to
- description
- created_at

### rules
- id (PK)
- tax_name
- ncm_code
- product_type
- base_formula
- rate
- fixed_value
- legal_ref
- conditions
- created_at

### rule_overrides
- id (PK)
- rule_id (FK -> rules.id)
- region
- customer_type
- rate_override
- fixed_override
- legal_ref
- conditions
- created_at

---

## 2. Logs Database (fw_taxex_logs.db)

### logs
- id (PK)
- invoice_number
- input_json
- output_json
- system_origin
- created_at

### audit
- id (PK)
- invoice_number
- rule_version
- rule_ids
- created_at
