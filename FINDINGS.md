# SauceDemo - Confirmed Functional Findings

State file for the bug-hunt loop. CONFIRMED functional findings only.
The hunter forgets between runs; this file does not.

---

## Confirmed findings

[CONFIRMED] problem_user inventory sorting does not work
Flow: Inventory sorting
Steps:
1. Log in as `problem_user` with the documented password.
2. Select Name (Z to A), Price (low to high), or Price (high to low).
Expected: Products reorder according to the selected sort.
Actual: The selection reverts to Name (A to Z) and products remain in A-to-Z order.
Evidence: Independent verifier reproduced the failure for `za`, `lohi`, and `hilo`.
First seen: 2026-06-27

[CONFIRMED] problem_user cannot add three products to the cart
Flow: Inventory add-to-cart
Steps:
1. Log in as `problem_user` with the documented password.
2. Click Add to cart for Sauce Labs Bolt T-Shirt, Sauce Labs Fleece Jacket, or Test.allTheThings() T-Shirt (Red).
Expected: The button changes to Remove, the cart badge increments, and the product appears in the cart.
Actual: The button remains Add to cart, the badge does not increment, and the product is not added.
Evidence: Independent verifier reproduced the failure for all three products; other product buttons worked.
First seen: 2026-06-27

[CONFIRMED] problem_user Last Name input overwrites First Name
Flow: Checkout information
Steps:
1. Log in as `problem_user` and add a working product to the cart.
2. Proceed to Checkout: Your Information.
3. Enter `Ada` in First Name and `Lovelace` in Last Name.
4. Click Continue.
Expected: Both names remain in their respective fields and checkout advances.
Actual: First Name becomes `Lovelace`, Last Name remains blank, and checkout is blocked with `Error: Last Name is required`.
Evidence: Independent verifier reproduced the field overwrite and validation error on checkout step one.
First seen: 2026-06-27

[CONFIRMED] error_user sorting raises an error and does not reorder products
Flow: Inventory sorting
Steps:
1. Log in as `error_user` with the documented password.
2. Select Name (Z to A).
Expected: Products reorder from Z to A.
Actual: An alert reports `Sorting is broken! This error has been reported to Backtrace.` and the selection/order remains A to Z.
Evidence: Independent verifier reproduced the alert and unchanged order.
First seen: 2026-06-27

[CONFIRMED] error_user cannot add three products to the cart
Flow: Inventory add-to-cart
Steps:
1. Log in as `error_user` with the documented password.
2. Click Add to cart for Sauce Labs Bolt T-Shirt, Sauce Labs Fleece Jacket, or Test.allTheThings() T-Shirt (Red).
Expected: The button changes to Remove, the cart badge increments, and the product appears in the cart.
Actual: The button remains Add to cart, the badge does not increment, and the product is not added.
Evidence: Independent verifier reproduced the failure for all three products.
First seen: 2026-06-27

[CONFIRMED] Reset App State leaves stale inventory button state
Flow: Session and app-state reset
Steps:
1. Log in as `standard_user` with an empty cart.
2. Add Sauce Labs Backpack and confirm the cart badge shows `1`.
3. Open the menu and click Reset App State.
Expected: The cart badge clears and the Backpack button immediately returns to Add to cart.
Actual: The badge clears, but the Backpack button remains Remove until the page is reloaded.
Evidence: Independent verifier reproduced the partial reset from a clean standard-user session; reload corrected the button and the cart remained empty.
First seen: 2026-06-27

[CONFIRMED] error_user checkout accepts a blank Last Name
Flow: Checkout information validation
Steps:
1. Log in as `error_user`, add a working product, and proceed to Checkout: Your Information.
2. Enter `Ada` in First Name, `Lovelace` in Last Name, and `12345` in Zip/Postal Code.
3. Click Continue.
Expected: Last Name retains `Lovelace`; otherwise checkout reports that Last Name is required.
Actual: Last Name remains blank, but checkout advances to the overview without a validation error.
Evidence: Independent verifier reproduced the blank Last Name and successful advance from a clean error-user session.
First seen: 2026-06-27

[CONFIRMED] error_user cannot finish checkout
Flow: Checkout completion
Steps:
1. Log in as `error_user`, add a working product, and proceed through checkout to the overview.
2. Click Finish.
Expected: Checkout advances to the completion page and shows the order confirmation.
Actual: The page remains on `checkout-step-two.html`; the overview stays visible and no completion message appears.
Evidence: Independent verifier reproduced the no-op Finish action from a clean checkout flow.
First seen: 2026-06-27

[CONFIRMED] visual_user product prices change across renders
Flow: Inventory pricing
Steps:
1. Log in as `visual_user` with the documented password.
2. Record the displayed product prices.
3. Reload the inventory page, then change the sort order.
Expected: Each product keeps a stable price.
Actual: Product prices change after reload and again after sorting.
Evidence: Independent verifier observed Backpack change from `$58.09` to `$80.91` to `$89.32` across the three renders.
First seen: 2026-06-27

[CONFIRMED] visual_user price sort ignores displayed prices
Flow: Inventory sorting
Steps:
1. Log in as `visual_user` with the documented password.
2. Select Price (low to high).
Expected: Displayed product prices appear in ascending numeric order.
Actual: Products are not ordered by their displayed prices.
Evidence: Independent verifier observed displayed prices `$3.97, $64.77, $35.51, $49.83, $47.41, $28.87` with low-to-high selected.
First seen: 2026-06-27

[CONFIRMED] visual_user inventory and cart prices disagree
Flow: Inventory-to-cart pricing
Steps:
1. Log in as `visual_user` with the documented password.
2. Note the displayed inventory price for Sauce Labs Backpack.
3. Add the Backpack to the cart and open the cart.
Expected: The cart price matches the inventory price shown when the item was added.
Actual: The cart shows a different price.
Evidence: Independent verifier observed `$14.98` on inventory and `$29.99` in the cart in the same flow.
First seen: 2026-06-27

## Observations
<!-- COSMETIC: real but non-blocking visual/asset issues. One line each. -->
[COSMETIC] problem_user shows the same placeholder image for every product - Inventory - all six products use the same `sl-404` image while inventory and cart actions remain usable (2026-06-27)

## Needs human
<!-- OUT_OF_SCOPE / security-adjacent candidates, refused and not investigated. One line each. -->
