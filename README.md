
Why Rust for embedded?
Rust for embedded makes the claim to be more memory safe than C, saving developers a lot of debugging time and raising awareness when reading from unitialized memory areas.  A couple language features that help create better hardware abstraction layers are intriguing.

traits: a means of defining what a type can do. For instance, in embedded systems, traits can describe which CPU pins can be configured as PWM outputs and how they should be configured. They are essentially the data sheet, encoded in Rust.
generics:  They tell Rust that a function can take anything it likes, just as long as the type implements certain traits. For example, generics enable a developer to write a function to which any pin can be passed, as long as the pin can be configured as a PWM output. We can write a single function that can control any given PWM pin.

This readme contains a brief description of how I used rust to generate an embedded Hello World for a Cortex-M3 processor.
QEMU was used to run the application since I do not have access to a Cortex-M3 development board.

The development board in the tuturial I followed(if I wanted to run on actual hardware) is an STM32F3Discovery Board.
I was actually quite how easy it was to create an application for embedded with Rust.

I chose VSCode as my editor because I am familiar with it, and in reading the documentation, there appears to be built-in   debugging support for rust.

Steps taken to get the hello world up and running:
1.  Used the rust book tutorial to get started.
    https://docs.rust-embedded.org/book/intro/index.html

2.  Forked the rust-embedded hello world example into my own github repo.
    https://github.com/rust-embedded/cortex-m-quickstart

3.  Cloned the newly forked repo onto my macbook
    git clone https://github.com/chadkidd/rust-hello-world

4.  Install rustup:
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

5.  Added cortex-m3 target
    rustup target add thumbv7m-none-eabi

6.  Updated author and application name in the cargo.toml file

7.  Built the application
    cargo build --target thumbv7m-none-eabi

8.  If successful, hello application now exists at:  target/thumbv7m-none-eabi/debug/examples/hello

9.  Installed Qemu - on Mac, this was as simple as:  
    brew install qemu

10. Ran the application with qemu
    qemu-system-arm \
      -cpu cortex-m3 \
      -machine lm3s6965evb \
      -nographic \
      -semihosting-config enable=on,target=native \
      -kernel target/thumbv7m-none-eabi/debug/examples/hello


Whala - hello world succeeds!


Next steps for investigation:
1.  Get a rust example working on an actual STM32F4 board, which I do happen to have in my collection.  See how easy it might be to add on external hardware / bring in other types of drivers for a more complex application.
2.  Learn more about how Rust might be used in embedded projects.  My appetite has been whetted with how easy it was to create a hello world application.  Try out the traits and generics.
3.  Attempt to get the VSCode debug capability working.