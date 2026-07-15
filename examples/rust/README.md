# Rust Example — cargo test

Rust's type system removes many classes of bugs, but code can still rely on hidden assumptions about ordering, state, precision, and public contracts.

## Code under test

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Score {
    pub user_id: String,
    pub points: i64,
}

pub fn top_score(scores: &[Score]) -> Option<Score> {
    scores.iter().max_by_key(|score| score.points).cloned()
}
```

## Hidden assumptions

```txt
Assumption A1: Tie behavior does not matter.
Evidence: max_by_key returns one of the max elements according to iterator behavior.
Risk: Equal scores may produce surprising or unstable product behavior.
Test idea: Provide equal points and assert documented tie policy.
Priority: Medium

Assumption A2: Negative scores are valid.
Evidence: points is i64 and no validation exists.
Risk: Some products may not allow negative points.
Test idea: Add test based on documented domain rule.
Priority: Medium

Assumption A3: Empty input is allowed.
Evidence: Return type is Option.
Risk: Caller may not handle None correctly.
Test idea: Verify empty input returns None and caller behavior handles it.
Priority: Low
```

## Example tests

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn returns_none_for_empty_scores() {
        assert_eq!(top_score(&[]), None);
    }

    #[test]
    fn returns_score_with_highest_points() {
        let scores = vec![
            Score { user_id: "u1".into(), points: 10 },
            Score { user_id: "u2".into(), points: 30 },
            Score { user_id: "u3".into(), points: 20 },
        ];

        assert_eq!(
            top_score(&scores),
            Some(Score { user_id: "u2".into(), points: 30 })
        );
    }

    #[test]
    fn tie_policy_is_explicit_when_scores_are_equal() {
        let scores = vec![
            Score { user_id: "first".into(), points: 30 },
            Score { user_id: "second".into(), points: 30 },
        ];

        // If the product expects first-wins, this test documents it.
        // If the product expects deterministic secondary sorting, change the contract and test.
        assert_eq!(
            top_score(&scores),
            Some(Score { user_id: "first".into(), points: 30 })
        );
    }
}
```

## Why these tests matter

The important test is not the obvious highest-score case. The important test is the tie case, because the code had an implicit product assumption about equal scores.

Assumption Forge makes that assumption visible.
