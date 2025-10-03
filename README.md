![BillJS](billjs.png "billjs")

# **billjs**

A lightweight TypeScript billing engine for invoices, receipts, and point-of-sale systems.  
 Supports item-level and global discounts, charges, configurable tax rules (inclusive/exclusive), safe rounding, and transparent calculation breakdowns.

---

## **✨ Features**

* Item-level & global discounts (fixed or percentage).

* Configurable charges (fixed or percentage, flexible base).

* Taxes (inclusive/exclusive, different applyOn bases).

* Safe rounding & consistent precision.

* Detailed breakdowns (items, charges, taxes, formulas).

* Auto-generated billing IDs.

---

## **🚀 Installation**

```bash
npm install billjs
```
---

## **📖 Quick Start**
```typescript
import { calculateBill, DiscountKind, ChargeKind } from "billjs";

const result \= calculateBill({  
  items: \[  
    {  
      name: "Laptop",  
      qty: 1,  
      unitPrice: 1000,  
      discount: { kind: DiscountKind.PERCENT, value: 10 }, // 10% off  
    },  
    { name: "Mouse", qty: 2, unitPrice: 50 },  
  \],  
  charges: \[  
    { name: "Delivery", kind: ChargeKind.FIXED, value: 20 },  
    { name: "Service Fee", kind: ChargeKind.PERCENT, value: 2 },  
  \],  
  taxes: \[  
    { name: "GST", rate: 18, inclusive: false, applyOn: "netAfterDiscount" },  
  \],  
  config: {  
    billingIdPrefix: "INV",  
    decimalPlaces: 2,  
    roundOff: true,  
  },  
});

console.log(result.total);       // → Final total  
console.log(result.items);       // → Per-item breakdown  
console.log(result.taxes);       // → Applied tax breakdown  
console.log(result.formula);     // → Human-readable steps
```
---

## **🧾 Concepts**

### **Items**

Each item has `qty × unitPrice`, with optional item-level discount and `taxFree` flag.

### **Discounts**

* **Item-level**: fixed or percentage off a single item.

* **Global**: applied on the subtotal (after item discounts).

discount: { kind: DiscountKind.FIXED, value: 50 }  
discount: { kind: DiscountKind.PERCENT, value: 10 }

### **Charges**

Additional costs (delivery, fees) applied on `subtotal`, `taxableBase`, or `netAfterDiscount`.

```typescript
{ name: "Service Fee", kind: ChargeKind.PERCENT, value: 5, applyOn: "subtotal" }
```
### **Taxes**

Flexible rules:

* `inclusive: true` → tax already included in price.

* `inclusive: false` → added on top.

* `applyOn`: `"taxableBase" | "subtotal" | "charges" | "netAfterDiscount"`.

---

## **📊 Example Scenario**

**Electronics shop invoice:**

* Items: Laptop (10% off), Mouse ×2.

* Charges: Delivery fee, Service fee (2%).

* Taxes: GST @18%.

* Global discount: none.

Result breakdown:

* Subtotal: 1100

* Discounts: 100

* Charges: \~42

* Taxes: \~180

* **Final Total ≈ 1222**

---

## **📌 Output Example**
```json
{  
  "billingId": "INV-20251003-123456-1234",  
  "subtotal": 1100,  
  "totalItemDiscount": 100,  
  "globalDiscount": 0,  
  "charges": \[  
    { "name": "Delivery", "amount": 20 },  
    { "name": "Service Fee", "amount": 22 }  
  \],  
  "taxes": \[  
    { "name": "GST", "rate": 18, "amount": 180 }  
  \],  
  "total": 1222  
}
```

---

## **🛠️ Use Cases**

* Invoices & receipts.

* POS and e-commerce checkout systems.

* SaaS billing & subscription breakdowns.

* Accounting and tax compliance.