<img src="assets/logo.png" width="88" height="88" alt="Blyx calibration logo">

# Blyx Calibration

Two models can make the same decisions and assign very different probabilities. Compare their binary probability forecasts with the Brier score, and inspect reliability bins to see where confidence and outcomes disagree.

## Run the example

Install the Blyx 0.2.1 CLI, Rust 1.85 or newer, and a system linker. From this repository:

```sh
blyxc native-run example.blyx
blyxc native-run tests.blyx
```

The example prints a Brier score of `0.0625` and a two-bin expected calibration error of `0.25`.

## Use in another Blyx project

```sh
git clone --depth 1 --branch v0.1.1 https://github.com/Rahulchaube1/blyx-calibration.git vendor/calibration
```

```blyx
use vendor::calibration::calibration;
fn main() {
    let labels = [0, 0, 1, 1];
    let scores = [0.25, 0.25, 0.75, 0.75];
    println!(cal_brier(labels, scores));
    println!(cal_ece(labels, scores, 2));
}
```

Save the program beside `vendor` and run `blyxc native-run main.blyx`. There are no package dependencies.

## API

| Function | Result |
| --- | --- |
| `cal_squared_error(label, score)` | Squared error for one validated binary outcome and probability. |
| `cal_brier(labels, scores)` | Mean squared probability error, in [0,1]. Lower is better. |
| `cal_bin(labels, scores, index, bins)` | Vector of count, sum of probabilities, and sum of outcomes. Divide each sum by count for a populated bin. |
| `cal_ece(labels, scores, bins)` | Count-weighted absolute gap between mean probability and observed positive frequency across equal-width bins. |

Labels must be 0 or 1. Scores must be finite and in [0,1]. Vectors must be nonempty and equally long. Choose 1 to 100 bins. Bins include their lower boundary, and the final bin includes 1. Invalid input returns `-1.0` for Brier or ECE and an empty vector for `cal_bin`. A valid empty bin returns `[0,0,0]`.

Brier and individual bin summaries take linear time. ECE makes one pass per bin and uses constant auxiliary space. Compare ECE at the same bin count and inspect counts before interpreting small-sample results.

[Blyx package catalog](https://www.blyx-lang.space/packages) · [Blyx compiler](https://github.com/Rahulchaube1/blyxxxx/releases/tag/v0.2.1)

## Contribute

Run the documented tests, then open a pull request with a small input that reproduces the behavior you want to change. Include the expected result and your Blyx compiler version. [Package issues](https://github.com/Rahulchaube1/blyx-calibration/issues) · [Ecosystem catalog](https://www.blyx-lang.space/packages)
