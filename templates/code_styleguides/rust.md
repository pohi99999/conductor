# Rust Style Guide Summary

This document summarizes key rules and best practices for writing idiomatic Rust code, based on the official Rust API Guidelines and community conventions.

## 1. Formatting
- **`rustfmt`:** All Rust code **must** be formatted with `rustfmt`. This is a non-negotiable, automated standard.
- **Indentation:** Use 4 spaces per indentation level (handled by `rustfmt`).
- **Line Length:** Maximum 100 characters (configurable in `rustfmt.toml`).
- **Braces:** Opening braces go on the same line. Closing braces go on their own line.

## 2. Naming
- **Types & Traits:** `UpperCamelCase` (e.g., `MyStruct`, `MyTrait`).
- **Functions & Methods:** `snake_case` (e.g., `my_function`, `calculate_total`).
- **Variables & Parameters:** `snake_case` (e.g., `user_name`, `item_count`).
- **Constants & Statics:** `SCREAMING_SNAKE_CASE` (e.g., `MAX_SIZE`, `DEFAULT_VALUE`).
- **Modules & Crates:** `snake_case` (e.g., `my_module`, `my_crate`).
- **Type Parameters:** Single uppercase letters or short `UpperCamelCase` (e.g., `T`, `E`, `Key`, `Value`).
- **Lifetimes:** Short lowercase names, typically `'a`, `'b`, or descriptive like `'src`.

## 3. Code Organization
- **Imports:** Group imports in the following order, separated by blank lines:
  1. Standard library (`std`)
  2. External crates
  3. Current crate modules (`crate::`, `super::`, `self::`)
- **Module Structure:** Prefer `mod.rs` or inline modules for small modules. Use separate files for larger modules.
- **Visibility:** Keep items private by default. Use `pub` only when necessary. Prefer `pub(crate)` or `pub(super)` over `pub` when possible.

## 4. Error Handling
- **`Result` Type:** Use `Result<T, E>` for recoverable errors. Define custom error types for libraries.
- **`?` Operator:** Use the `?` operator for propagating errors instead of explicit `match` or `unwrap`.
- **`unwrap()` / `expect()`:** Avoid in production code. Use `expect("descriptive message")` if a panic is truly intended.
- **`panic!`:** Reserve for unrecoverable errors or programming bugs. Document when functions can panic.

## 5. Ownership & Borrowing
- **Prefer Borrowing:** Borrow (`&T` or `&mut T`) instead of taking ownership when possible.
- **Clone Sparingly:** Avoid unnecessary `.clone()` calls. Consider restructuring code to avoid cloning.
- **Lifetimes:** Elide lifetimes when possible. Add explicit lifetimes only when the compiler requires them.
- **Smart Pointers:** Use `Box<T>` for heap allocation, `Rc<T>` for shared ownership, `Arc<T>` for thread-safe shared ownership.

## 6. Types & Traits
- **Type Inference:** Let the compiler infer types when obvious. Be explicit when it aids readability.
- **Generics:** Prefer generics over trait objects (`dyn Trait`) for performance-critical code.
- **Trait Bounds:** Use `where` clauses for complex bounds to improve readability.
- **Derive Macros:** Use `#[derive(...)]` for common traits: `Debug`, `Clone`, `Copy`, `PartialEq`, `Eq`, `Hash`, `Default`.
- **`impl` Blocks:** Group related methods in a single `impl` block. Separate trait implementations.

## 7. Documentation
- **Doc Comments:** Use `///` for item documentation. Use `//!` for module-level documentation.
- **Examples:** Include runnable examples in doc comments using triple backticks with `rust`.
- **Sections:** Use `# Examples`, `# Panics`, `# Errors`, `# Safety` sections where appropriate.
- **Links:** Link to related items using `[`brackets`]` syntax.

```rust
/// Calculates the factorial of a number.
///
/// # Examples
///
/// ```
/// let result = my_crate::factorial(5);
/// assert_eq!(result, 120);
/// ```
///
/// # Panics
///
/// Panics if `n` is greater than 20 (overflow).
pub fn factorial(n: u64) -> u64 {
    // implementation
}
```

## 8. Safety & Unsafe Code
- **Minimize `unsafe`:** Use `unsafe` blocks only when absolutely necessary.
- **Document Invariants:** Every `unsafe` block must have a `// SAFETY:` comment explaining why it's safe.
- **Encapsulate:** Wrap unsafe code in safe abstractions with well-documented safety requirements.

## 9. Concurrency
- **Prefer Channels:** Use `std::sync::mpsc` or `crossbeam` channels for message passing.
- **Mutex & RwLock:** Use `Mutex<T>` for exclusive access, `RwLock<T>` for multiple readers.
- **Avoid Deadlocks:** Always acquire locks in a consistent order. Prefer scoped locks.
- **`Send` & `Sync`:** Understand these marker traits. Most types are automatically `Send` and `Sync`.

## 10. Testing
- **Unit Tests:** Place in a `#[cfg(test)]` module at the bottom of the source file.
- **Integration Tests:** Place in a `tests/` directory at the crate root.
- **Test Naming:** Use descriptive names: `test_function_name_condition_expected_result`.
- **Assertions:** Prefer `assert_eq!` and `assert_ne!` over `assert!` for better error messages.

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_factorial_of_five_returns_120() {
        assert_eq!(factorial(5), 120);
    }

    #[test]
    #[should_panic(expected = "overflow")]
    fn test_factorial_overflow_panics() {
        factorial(21);
    }
}
```

## 11. Performance
- **Iterators:** Prefer iterators over index-based loops. They are often zero-cost abstractions.
- **Collect Wisely:** Specify the collection type when using `.collect()` (e.g., `.collect::<Vec<_>>()`).
- **Avoid Allocations:** Reuse buffers when possible. Use `&str` instead of `String` for read-only strings.
- **Benchmarks:** Use `criterion` or the built-in benchmark framework for performance testing.

## 12. Clippy Lints
- **Run Clippy:** Always run `cargo clippy` before committing code.
- **Fix Warnings:** Address all Clippy warnings. Use `#[allow(...)]` sparingly and with justification.
- **Pedantic Lints:** Consider enabling `#![warn(clippy::pedantic)]` for stricter checks.

## 13. Common Idioms
- **Builder Pattern:** Use for structs with many optional fields.
- **Newtype Pattern:** Wrap primitive types for type safety (e.g., `struct UserId(u64)`).
- **From/Into:** Implement `From<T>` for type conversions; `Into` is automatically provided.
- **Default:** Implement `Default` for types with sensible default values.
- **Display & Debug:** Implement `Display` for user-facing output, `Debug` for developer output.

**BE CONSISTENT.** When editing existing code, match the existing style.

*Sources:*
- [The Rust Programming Language Book](https://doc.rust-lang.org/book/)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- [Rust Style Guide](https://doc.rust-lang.org/nightly/style-guide/)
