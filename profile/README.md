# knurling-rs

> Get a handle on bare-metal Rust

Knurling-rs is a project by [Ferrous Systems](http://ferrous-systems.com/). Our mission is to improve the embedded Rust experience. To achieve this, we build and improve tools and create learning materials.

We believe that developing for embedded systems should be no more difficult than developing for hosted platforms.

## Tools

It should be possible to use the same workflows and equally powerful tooling to develop applications and libraries for tiny embedded systems as well as powerful server-class hardware.

To that end we have created following tools:

<details>
<summary><b>defmt</b></summary>

[`defmt`] is a highly efficient logging framework that targets resource-constrained devices, like microcontrollers. `defmt` stands for "deferred formatting".

Rather than performing formatting input like `255u8` into `"The temperature is 255°C"` on the embedded device, the formatting is deferred to the host that will output the logs. This way, only the relevant data needs to be sent to the host instead of the entire format string. Additionally the transmitted data is compressed, for example by [compressing several booleans into one byte].

This means that `defmt`:
- reduces compiled binary size, since it only has to contain indices to log strings saved by the host instead of the string itself
- reduces computation efforts on the target, because the formatting of e.g. `255u8` to `"255"` happens on the host that displays the logging output, not the target
- reduces delays and log buffer usage, since there is less data sent from the embedded device to the host

For more details on how it works, check out the [`defmt` book].

[`defmt`]: https://github.com/knurling-rs/defmt
[compressing several booleans into one byte]: https://defmt.ferrous-systems.com/ser-bool.html
[`defmt` book]: https://defmt.ferrous-systems.com/

</details>
<details>
<summary><b>flip-link</b></summary>

[`flip-link`] adds zero-cost stack overflow protection for your embedded programs – no MPU or stack probe support needed!

It does this by [flipping the standard memory layout of ARM Cortex-M programs].

With this inverted memory layout, the stack "overflows" instead of corrupting memory when it hits the boundary of the RAM region. This boundary collision raises a hardware exception (usually the "hard fault" exception), which by default halts the program.

For more details, check out our [blog post introducing `flip-link`][changelog-1].

[`flip-link`]: https://github.com/knurling-rs/flip-link
[flipping the standard memory layout of ARM Cortex-M programs]: https://blog.japaric.io/stack-overflow-protection/
[changelog-1]: https://ferrous-systems.com/blog/knurling-changelog-1/

</details>
<details>
<summary><b>app-template</b></summary>

The [`app-template`] is a Cargo project template, so you can hit the ground running with `probe-run`, `defmt` and `flip-link`. Using the knurling `app-template`, and [`cargo-generate`], you can start your embedded project by just running

```console
$ cargo generate \
    --git https://github.com/knurling-rs/app-template \
    --branch main \
    --name my-app
```

and specifying your desired HAL and compilation target.

[`app-template`]: https://github.com/knurling-rs/app-template
[`cargo-generate`]: https://github.com/ashleygwilliams/cargo-generate

</details>
<details>
<summary><b>defmt-test</b></summary>

[`defmt-test`] is an embedded test harness that lets you write and run *unit tests* as if you were using the built-in `#[test]` attribute, but they'll run on your embedded target.

Of course, `defmt-test` also gives you an `#[init]` attribute for initialization functions needed to set up your peripherals etc.

For more details, check out our [blog post introducing `defmt-test`][changelog-1]. Also check our [blog post series on testing embedded Rust code](https://ferrous-systems.com/blog/tags/embedded-rust-testing/).

[`defmt-test`]: https://github.com/knurling-rs/defmt/tree/main/firmware/defmt-test

</details>

## Learning materials

Sometimes we create learning resources to help newcomers to embedded Rust to get their hands dirty

| name | website | repository |
| :--  | :--     | :--        |
| Knurling Session 2020 "Build a C02 measuring device" | https://session20q4.ferrous-systems.com/ | https://github.com/knurling-rs/knurling-session-20q4 |

## Blog

You can find some project updates and behind the scenes insights in the [Ferrous Systems Blog](https://ferrous-systems.com/blog/tags/knurling-rs/).
