# 📦 FWTaxEx

> FWTaxEx is an open-source, extensible **Tax Engine in Harbour**, designed to handle Brazilian tax calculation (and adaptable to other countries).  
> It decouples tax rules from ERPs, consuming **JSON invoices** and applying **versioned SQLite rules** to return calculated taxes in a standard format.

## ✨ Features

- 🔹 **Engine-based design**: fully object-oriented in Harbour.  
- 🔹 **Versioned rules**: each SQLite database represents a tax rule version.  
- 🔹 **ERP agnostic**: works with Protheus, Datasul, SAP, or any system via JSON.  
- 🔹 **Legal compliance**: rules linked to official law acts.  
- 🔹 **Audit ready**: logs stored in a separate SQLite database for traceability.  

## 🛠 Directory Structure

```
fw.taxex/
│
├── src/                        # OO source code
│   ├── core/                   # Main engine
│   ├── adapters/               # ERP connectors (Protheus, REST, etc.)
│   └── utils/                  # Helpers
│
├── db/                         # Versioned SQLite databases
├── config/                     # External configs (JSON)
├── docs/                       # Documentation
├── tests/                      # Unit tests
└── examples/                   # Usage examples
```

## 🚀 Quick Example

### Input JSON (invoice data)

```json
{
  "company_id": "12345678000199",
  "invoice_number": "NF12345",
  "items": [
    {
      "ncm": "22021000",
      "description": "Mineral Water",
      "quantity": 100,
      "unit_price": 2.50
    }
  ]
}
```

### Harbour Usage

```harbour
LOCAL oEngine   := FWTaxExEngine():New()
LOCAL cJSON     := MemoRead("invoice.json")
LOCAL oResult   := oEngine:CalcFromJSON(cJSON)

? hb_jsonEncode(oResult)
```

### Output JSON (calculated taxes)

```json
{
  "invoice_number": "NF12345",
  "calculated_taxes": [
    {
      "tax": "ICMS",
      "base": 250.00,
      "rate": 0.18,
      "value": 45.00
    },
    {
      "tax": "PIS",
      "base": 250.00,
      "rate": 0.0165,
      "value": 4.13
    }
  ]
}
```

## 📚 Documentation

- [ARCHITECTURE.md](./docs/ARCHITECTURE.md) → OO class design  
- [DB_SCHEMA.md](./docs/DB_SCHEMA.md) → SQLite schema for rules & logs  

## 🤝 Contributing

PRs and issues are welcome!  
The goal is to build a **universal, community-driven tax engine** that eliminates the need for companies to configure tax rules on their own.  

⚖️ **Disclaimer**: This project provides a calculation engine based on public tax rules. Responsibility for compliance remains with the user/company.
