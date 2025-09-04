# Restaurant Menu DTD

A minimal XML + DTD package for validating a small restaurant menu.

## Files
- `domain.dtd` — minimal schema (required elements, order, and some enumerations).
- `sample.xml` — **valid** XML that passes DTD validation.
- `broken.xml` — **invalid** XML that fails DTD validation.
- `validate.ipynb` — Colab-friendly notebook that validates both files and prints results.

## How to run locally
```bash
# Using xmllint
xmllint --noout --dtdvalid domain.dtd sample.xml
xmllint --noout --dtdvalid domain.dtd broken.xml
```

Or open `validate.ipynb` in Google Colab and run all cells.

---

## Framework

**Decision: What must be answered/acted?**  
Guests and staff need to know **what items are available today at what price**, and whether they **fit dietary needs**.

**Signals (tagged to Data Domains):**  
- [Restaurant Menu] **Item count** (`item+`), refreshed daily via `lastUpdated (YYYY-MM-DD)`.  
- [Restaurant Menu] **Availability** per item (`availability@status ∈ {available, soldout, seasonal}`).  
- [Payments] **Price** per item (`price@currency`, numeric amount in file currency).  
- [Nutrition] **Calories** per item (`calories` in kcal; optional but recommended).  
- [Allergens/Nutrition] **Allergens** list (`allergens/allergen+`).  
- [Restaurant Menu] **Category** (`item@category ∈ {appetizer, entree, dessert, beverage}`).  
- [Restaurant Menu] **Spicy level** (`item@spicy ∈ {none, mild, medium, hot}`).

**Gaps: What’s missing that would change confidence?**  
- No numeric type enforcement for `price`/`calories` in DTD (text only).  
- `lastUpdated` is free text (no date regex in DTD).  
- Portion size and tax rules are not modeled.  
- No localization (e.g., multilingual names) or per-branch overrides.

**Risk: Harm if wrong or stale.**  
- Mispriced or unavailable items cause guest frustration, refunds, and lost sales.  
- Missing allergen data can trigger health incidents.  
- Stale `lastUpdated` misleads guests about availability.

**Stewardship: Who can see/keep it, and for how long?**  
- Public menu data is visible to all; allergen data is public but must be accurate.  
- Edit rights restricted to restaurant staff; versioned for audit for **1 year**.

**Data Domain:**  
- **Primary:** [Restaurant Menu]  
- **Adjacent:** [Payments], [Nutrition]

**Broken XML – why it fails:**  
- *Violation:* `item@category="snack"` is not permitted by the DTD enumeration **and** the required `<availability/>` element is missing.

---

## GitHub / Colab
- Add these files to a repo, then include the repo link in your class discussion post.
- In your post, copy/paste the **Framework** section above.
- One-sentence failure reason (example): “Violation: `category` must be one of appetizer|entree|dessert|beverage; also missing `<availability/>`.”
