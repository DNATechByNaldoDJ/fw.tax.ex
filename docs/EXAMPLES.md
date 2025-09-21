# 📝 FWTaxEx – Examples

## Example 1: ICMS Calculation

### Input JSON

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

### Output JSON

```json
{
  "invoice_number": "NF12345",
  "calculated_taxes": [
    {
      "tax": "ICMS",
      "base": 250.00,
      "rate": 0.18,
      "value": 45.00
    }
  ]
}
```

### Harbour Usage

```harbour
LOCAL oEngine := FWTaxExEngine():New()
LOCAL cJSON   := MemoRead("invoice_icms.json")
LOCAL oResult := oEngine:CalcFromJSON(cJSON)

? hb_jsonEncode(oResult)
```

---

## Example 2: Using Mapper

```harbour
LOCAL oMapper := FWTaxExMapper():New()
LOCAL cERPJson := MemoRead("protheus_invoice.json")
LOCAL cStdJSON := oMapper:MapFromProtheus(cERPJson)

LOCAL oEngine := FWTaxExEngine():New()
LOCAL oResult := oEngine:CalcFromJSON(cStdJSON)

? hb_jsonEncode(oResult)
