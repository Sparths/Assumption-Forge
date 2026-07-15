# Python Example — pytest

This example shows how Assumption Forge thinks about tests before writing them.

## Code under test

```python
def apply_discount(amount_cents: int, discount_percent: int) -> int:
    return amount_cents - int(amount_cents * discount_percent / 100)
```

## Hidden assumptions

```txt
Assumption A1: amount_cents is always non-negative.
Evidence: No validation before arithmetic.
Risk: Negative charges may produce unexpected credits.
Test idea: Pass a negative amount and verify explicit behavior.
Priority: High

Assumption A2: discount_percent is between 0 and 100.
Evidence: The function accepts any integer.
Risk: Discounts above 100 produce negative totals; negative discounts increase price.
Test idea: Pass -1 and 101 and verify rejection or documented behavior.
Priority: High

Assumption A3: integer rounding is acceptable.
Evidence: int() truncates fractional cents.
Risk: Billing totals may be off by one cent.
Test idea: Use values where percentage creates fractional cents and assert expected rounding policy.
Priority: High
```

## Example tests

```python
import pytest

from billing import apply_discount


def test_rejects_discount_above_one_hundred_percent():
    with pytest.raises(ValueError, match="discount_percent"):
        apply_discount(10_00, 101)


def test_rejects_negative_discount_percent():
    with pytest.raises(ValueError, match="discount_percent"):
        apply_discount(10_00, -1)


def test_rejects_negative_amount_cents():
    with pytest.raises(ValueError, match="amount_cents"):
        apply_discount(-10_00, 10)


def test_rounding_policy_is_explicit_for_fractional_cents():
    # 999 cents with a 15% discount creates 149.85 cents of discount.
    # The expected result should match the product's documented rounding policy.
    assert apply_discount(999, 15) == 849
```

## Why these tests matter

The tests do not merely cover the arithmetic line. They protect the assumptions that billing code depends on: valid money values, bounded discounts, and explicit rounding behavior.

If the product actually allows negative amounts for credits, the test should change. The important part is that the behavior becomes explicit instead of accidental.
