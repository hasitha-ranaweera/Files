
# ✅ **Top-Level (EntityMetadata) Properties**

### **@odata.type**

* Identifies the payload as an `EntityMetadata` object.
* Required for Dataverse Web API to understand this is a table-definition request.

---

### **SchemaName**

* The *internal* .NET-style schema name for the table.
* Must be PascalCase and include your publisher prefix (`IA_`).
* Used in solutions, plugin code, form XML, etc.
* **Cannot be changed after creation**.

---

### **LogicalName**

* The underlying table name in the database (`ia_universalentity`).
* Used in Web API routes:

  * `/api/data/v9.2/ia_universalentities`
* Must be lowercase and follow prefix conventions.
* **Permanent once created**.

---

### **OwnershipType**

* Defines how records are owned.
* `"UserOwned"` → each row belongs to a user/team, enabling:

  * Owner column
  * Standard security roles (User vs Team)
  * Very common for functional tables.
* Alternatives: `OrganizationOwned`, `None`, etc.

---

### **HasActivities**

* `true` lets this entity have activity records (emails, tasks, appointments).
* For most custom tables, set to `false`.

---

### **HasNotes**

* Enables the built-in Notes (Annotations) capability.
* Equivalent to turning on the “Notes” toggle in table settings.
* Creates the **Related → Notes** panel automatically.

---

### **IsActivity**

* Defines whether the entity *is* an Activity type (like Email, Phone Call).
* Activity tables behave differently (regarding timelines, ownership, scheduling).
* Should always be `false` for normal tables.

---

### **Description**

Metadata object describing the table. Only visible in maker portal or solution explorers.

* `Label`: table description
* `LanguageCode`: 1033 = English (US)

---

### **DisplayName**

* Singular display name.
* Shown in UI, lookup dialogs, model-driven app navigation.

---

### **DisplayCollectionName**

* Plural display name.
* Shown in UI navigation (e.g., “Accounts”, “Contacts”).

---

---

# ✅ **Attributes (Column Definitions)**

The `"Attributes"` array defines the columns to create during table creation.

Below is the breakdown of your **StringAttributeMetadata** for the primary name column.

---

## 📌 Attribute Object-Level: `StringAttributeMetadata`

### **@odata.type**

* Type of column metadata object.
* Determines the supported properties.
* Other examples:

  * `BooleanAttributeMetadata`
  * `MoneyAttributeMetadata`
  * `PicklistAttributeMetadata` (Option Set)
  * etc.

---

### **SchemaName**

Internal name for the column used in metadata.

* Follows PublisherPrefix + PascalCase format.
* Used in plugin code, solution XML, etc.

---

### **LogicalName**

Actual column name stored in the database.

* Must be lowercase.
* Used in Web API:

  * `ia_entityid`

> **Cannot be changed later.**

---

### **AttributeType**

Logical type of the column (`String`).

---

### **AttributeTypeName**

More specific typed value wrapper:

```json
"AttributeTypeName": { "Value": "StringType" }
```

* Required for Web API metadata creation.
* Must match the correct attribute type name.

---

### **AutoNumberFormat**

* Turns this string column into an *Auto-Numbering* field.
* `EN-{SEQNUM:6}` → Example output: `EN-000123`
* Applied only at creation (cannot enable auto-numbering later through API for an existing string column).

---

### **DisplayName**

Label used in forms, views, and UI.

---

### **Description**

Tooltip and metadata description of the column.

---

### **IsPrimaryName**

* `true` = this column becomes the **Primary Name Column**.
* Every UserOwned table must have a primary name (string-only).
* Used in lookups, views, and search.

---

### **RequiredLevel**

Defines field requirement rules.

```json
"RequiredLevel": {
  "Value": "ApplicationRequired",
  "CanBeChanged": true,
  "ManagedPropertyLogicalName": "canmodifyrequirementlevelsettings"
}
```

* `SystemRequired` → cannot change
* `ApplicationRequired` → required at app level but can be changed
* `Recommended`
* `None`

`CanBeChanged: true` means makers can adjust later.

---

### **MaxLength**

* Maximum allowable characters for the String column.
* For auto-number, keep high enough for prefix + number.
* Max possible for string is usually 4000.

---

# Summary Table

| Property              | Meaning / Usage                            |
| --------------------- | ------------------------------------------ |
| SchemaName            | Internal schema name; permanent            |
| LogicalName           | Database-level name; used in APIs          |
| DisplayName           | UI label (singular)                        |
| DisplayCollectionName | UI plural label                            |
| HasNotes              | Enables Notes/Attachments                  |
| AutoNumberFormat      | Defines automatic numbering pattern        |
| RequiredLevel         | Required / Recommended / Optional          |
| IsPrimaryName         | Primary search/display column in Dataverse |
| MaxLength             | Character limit                            |
