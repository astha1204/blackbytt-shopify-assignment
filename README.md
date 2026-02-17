# Smart Delivery Note – Shopify Assignment

## Feature Overview

This implementation adds a **Delivery Note textarea** on the product page that appears only when the cart total exceeds ₹999.

The note:
- Is stored using **cart attributes**
- Appears on the **Cart page**
- Persists to **Checkout**
- Is visible in **Shopify Admin → Order → Additional Details**

No apps were used.  
Built using Liquid + Vanilla JavaScript on the Dawn theme.

---

# Files Modified

## 1. snippets/buy-buttons.liquid

### Changes:
- Added Delivery Note textarea inside:
  <div class="product-form__buttons">
- Used:
  name="attributes[Delivery Note]"
- Added maxlength (120 characters)
- Added live character counter
- Kept textarea hidden by default

Purpose:
Stores delivery note as a cart-level attribute.

---

## 2. sections/main-product.liquid

### Changes:
Added JavaScript before `{% endschema %}` that:

- Fetches `/cart.js`
- Checks `cart.total_price`
- Shows textarea if total ≥ ₹999 (99900 paise)
- Hides it otherwise
- Re-checks on:
  - Page load
  - Add-to-cart submit
  - Async cart update

Purpose:
Ensures textarea visibility always matches actual cart total.

---

## 3. sections/main-cart-items.liquid

### Changes:
Added code to display delivery note on the Cart page:

```liquid
{% if cart.attributes["Delivery Note"] != blank %}
  <div class="cart-delivery-note">
    <h3>Delivery Note</h3>
    <p>{{ cart.attributes["Delivery Note"] }}</p>
  </div>
{% endif %}
```
  Purpose:
Shows saved delivery note on Cart page before checkout.

---

# Why Cart Attributes Were Chosen

Cart attributes were used because:

- The delivery note applies to the entire order, not a single product.
- It persists from product page → cart → checkout.
- It appears in Shopify Admin under **Order → Additional Details**.
- It is the correct architectural choice for order-level data.

Line item properties were not used because they attach to individual products and would not represent an order-level note correctly.

---

# AI Assistance

AI (ChatGPT GPT-5) was used for:

- Structuring the conditional visibility logic
- Confirming correct use of `/cart.js`
- Debugging AJAX cart re-render issues
- Refining the paise comparison logic (₹999 → 99900)

Human judgment was required for:

- Choosing cart attributes over line item properties
- Ensuring Dawn theme compatibility
- Handling edge cases across page reload and async cart updates
- Avoiding over-engineering the solution

---

# Debug Scenario

Issue:
On some themes, the delivery note disappears after quantity change.

Possible Cause:
Many Shopify themes use AJAX-based section rendering.  
When cart updates dynamically, the DOM gets re-rendered and custom JS bindings are lost.

Fix Applied:
- Re-validate cart total on `DOMContentLoaded`
- Re-check after add-to-cart submit
- Add delayed re-check for async updates
- Ensure visibility logic depends strictly on `/cart.js` total_price

This ensures the textarea always reflects the actual cart state.

---

# UX Improvement (Next Iteration)

A future enhancement could include:

- A dynamic helper message:
  "Add ₹X more to unlock delivery note."
- Smooth animation when textarea appears
- Inline validation styling for accessibility

---

# Final Summary

✔ Built using Liquid + Vanilla JavaScript  
✔ No apps used  
✔ Dawn-compatible  
✔ Cart-level persistence achieved  
✔ Admin-visible order data  
✔ Clean and scalable implementation

