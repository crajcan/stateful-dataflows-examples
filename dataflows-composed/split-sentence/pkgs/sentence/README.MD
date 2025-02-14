# Sentence Package

The `sentence` package offers a couple of functions to work with sentences:

* sentence-to-workds - splits a sentence into individual words.
* augment-count - augments the sentence with the number of charaters.

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

Update the code for the APIs defined.

Replace the `sentence_to_words` function body in the `rust/sentence-to-words/src/lib.rs` with the following code:

```rust
fn sentence_to_words(sentence: String) -> Result<Vec<String>, String> {
    Ok(sentence.split_whitespace().map(String::from).collect())
}
```

In `rust/augment-count/src/lib.rs` replace the the function and add tests:

```rust
impl GuestInterface for Component {
    fn augment_count(word: String) -> Result<String, String> {
        Ok(format!("{}({})", word, word.chars().count()))
    }
}

#[cfg(test)]
mod test {
    use super::*;

    #[test]
    fn test_augment_count() {
        let input = "Hello".to_string();
        let output = Component::augment_count(input);
        assert_eq!(output.unwrap(), "Hello(5)");
    }
}
```

### Build the Package

To build the code, run:

```bash
ssdk build
```

### Test the Functions

Test the code with using the `ssdk` toolchain:

```bash
ssdk test
```

Show the functions avaialble for testing:

```bash
>> show functions
sentence-to-words
augment-count
```

Test `sentence-to-words` function:

```bash
>> test function sentence-to-words --input "Hello World"
Hello
World
```

Test `augment-count` function:

```bash
>> test function augment-count --input "Hello"
Hello(5)
```

To test the `rust` code via Cargo, perform the following steps:

```bash
cd rust/augment-count
cargo test
```


The package is ready to use in the [dataflow](../../).

[Install SSDK]: /README.MD#prerequisites