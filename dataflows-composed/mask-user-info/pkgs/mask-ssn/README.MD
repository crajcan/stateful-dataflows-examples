# Mask Social Security Number (SSN) Package

This is an example of building and testing a package that masks Social Security Numbers (SSNs) in a record.


### Prerequisites

* Checkout the [ssdk-package.yaml](./ssdk-package.yaml).
* Make sure to [Install SSDK].


### Generate the Package Sandbox

Use the `generate` scaffold to create the package sandbox:

```bash
ssdk generate
```

The generator creates all necessary glue logic for a reusable package.


### Add Custom Code

Let's update the content of `rust/mask-ssn/src/lib.rs` file that has been generated with the following code:

```rust
mod bindings;

use crate::bindings::exports::example::mask_ssn::mask_ssn_service::Guest as GuestInterface;
use regex::Regex;
use once_cell::sync::Lazy;

struct Component;

bindings::export!(Component with_types_in bindings);

static SSN_RE: Lazy<Regex> = Lazy::new(|| Regex::new(r"\d{3}-\d{2}-\d{4}").unwrap());

impl GuestInterface for Component {
    fn mask_ssn(text: String) -> Result<String, String> {
        let output = SSN_RE.replace_all(text.as_str(), "***-**-****").to_string();
        Ok(output)
    }   
}

#[cfg(test)]
mod test {
    use super::*;

    #[test]
    fn test_mask_ssn() {
        let input = "123-45-6789".to_string();
        let output = Component::mask_ssn(input);
        assert_eq!(output.unwrap(), "***-**-****");
    }
}
```

Next, add `once_cell` and `regex` to the dependencies in `Cargo.toml`:

```toml
[dependencies]
once_cell = "1"
regex = "1"
```

### Build the Package

To build the code, run:

```bash
ssdk build
```

**Tips**: In rust you may use `cargo component build` to compile, then `cargo test` to run the tests.


### Test the Code

Test the code with:

```bash
ssdk test
```

```bash
>> test function mask-ssn --input "My SSN is 534-34-3443."
My SSN is ***-**-****.
```

The package is ready to use in the [dataflow](../../).



[Install SSDK]: /README.MD#prerequisites