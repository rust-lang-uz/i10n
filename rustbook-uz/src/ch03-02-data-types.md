## Ma'lumotlar turlari

Rust-dagi har bir qiymat ma'lum bir *ma'lumot turiga* tegishli bo'lib, Rustga qanday ma'lumotlar ko'rsatilayotganligini bildiradi, shuning uchun u ushbu ma'lumotlar bilan qanday ishlashni biladi. Biz ikkita ma'lumotlar turini ko'rib chiqamiz: skalyar va birikma.

Esda tutingki, Rust *statik tarzda yozilgan* tildir, ya'ni kompilyatsiya vaqtida barcha o'zgaruvchilarning turlarini bilishi kerak. Kompilyator odatda qiymat va uni qanday ishlatishimiz asosida biz qaysi turdan foydalanmoqchi ekanligimiz haqida xulosa chiqarishi mumkin.
Ko‘p turlar mumkin bo‘lgan hollarda, masalan, 2-bobdagi [“Tahminni maxfiy raqam bilan solishtirish”][comparing-the-guess-to-the-secret-number]<!-- ignore --> bo‘limidagi `parse` yordamida `String`ni raqamli turga o‘zgartirganimizda, quyidagi turdagi izohni qo‘shishimiz kerak:

```rust
let taxmin: u32 = "42".parse().expect("Raqam emas!");
```

Oldingi kodda ko'rsatilgan `: u32` turidagi izohni qo'shmasak, Rust quyidagi xatoni ko'rsatadi, ya'ni kompilyator bizdan qaysi turdan foydalanishni xohlayotganimizni bilish uchun qo'shimcha ma'lumotga muhtoj:

```console
$ cargo build
   Compiling no_type_annotations v0.1.0 (file:///projects/no_type_annotations)
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let taxmin = "42".parse().expect("Not a number!");
  |         ^^^^^ consider giving `guess` a type

For more information about this error, try `rustc --explain E0282`.
error: could not compile `no_type_annotations` due to previous error
```

Boshqa ma'lumotlar turlari uchun turli turdagi izohlarni ko'rasiz.

### Skalyar Turlar

*Skalyar* turi bitta qiymatni ifodalaydi. Rust to'rtta asosiy skalyar turga ega: integerlar, floating-point number, boolean va belgilar. Siz ularni boshqa dasturlash tillaridan bilishingiz mumkin. Keling, ularning Rustda qanday ishlashini ko'rib chiqaylik.

#### Integer Turlari

*Integer* kasr komponenti bo‘lmagan sondir. Biz 2-bobda `u32` tipidagi bitta *integer* sonni ishlatdik. Ushbu turdagi deklaratsiya u bilan bog'langan qiymat 32 bit bo'sh joyni egallagan belgisiz butun son bo'lishi kerakligini bildiradi (Signed integer sonlar `u` o'rniga `i` bilan boshlanadi). 3-1-jadvalda Rust-da o'rnatilgan integer son turlari ko'rsatilgan. Integer son qiymatining turini e'lon qilish uchun biz ushbu variantlardan foydalanishimiz mumkin.

<span class="caption">3-1-jadval: Rustdagi Integer sonlar turlari</span>

| Uzunlik | Signed  | Unsigned |
|---------|---------|----------|
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

Signedlar kichkini `i` harfi bilan boshlanadi, Unsigned esa kichik `u` harfi bilan boshlanadi.

Har bir variant signed yoki unsigned bo'lishi mumkin va aniq o'lchamga ega.
*Signed* va *Unsigned* raqam manfiy boʻlishi mumkinmi yoki yoʻqligini anglatadi, boshqacha qilib aytganda, raqam u bilan birga belgiga ega boʻlishi (signed) boʻlishi kerakmi yoki u faqat ijobiy bo'ladimi va shuning uchun belgisiz (unsigned) ifodalanishi mumkinmi. Bu raqamlarni qog'ozga yozishga o'xshaydi: belgi muhim bo'lsa, raqam ortiqcha yoki minus belgisi bilan ko'rsatiladi; ammo, agar raqamni ijobiy deb hisoblash xavfsiz bo'lsa, u hech qanday belgisiz ko'rsatiladi.
Signed raqamlar [ikkita to'ldiruvchi][twos-complement]<!-- ignore--> ko'rinish yordamida saqlanadi.


Har bir signed variant -(2<sup>n - 1</sup>) dan 2<sup>n -
1</sup> -1 gacha bo'lgan raqamlarni saqlashi mumkin, bu erda *n* variant foydalanadigan bitlar soni.
Shunday qilib, `i8` -(2<sup>7</sup>) dan 2<sup>7</sup> - 1, gacha bo'lgan raqamlarni saqlashi mumkin, bu tengdir -128 dan 127 gacha.
Unsigned variantlar 0 dan 2<sup>n</sup> - 1 gacha raqamlarni saqlashi mumkin, shuning uchun `u8` 0 dan 2<sup>8</sup> - 1 gacha bo'lgan raqamlarni saqlashi mumkin, bu 0 dan 255 gacha.

Bundan tashqari, `isize` va `usize` turlari dasturingiz ishlayotgan kompyuterning arxitekturasiga bog'liq bo'lib, u jadvalda “arch” sifatida ko'rsatilgan: agar siz 64 bitli arxitekturada bo'lsangiz 64 bit va 32 bitli arxitekturada bo'lsangiz 32 bit.

Integer sonlarni 3-2-jadvalda ko'rsatilgan istalgan shaklda yozishingiz mumkin. E'tibor bering, bir nechta raqamli turlar bo'lishi mumkin bo'lgan son harflari turni belgilash uchun `57u8` kabi tur qo'shimchasiga ruxsat beradi. Raqamni o'qishni osonlashtirish uchun `_` dan raqamli harflar ham foydalanishi mumkin, masalan, `1_000`, siz `1000` ni ko'rsatganingizdek bir xil qiymatga ega bo'ladi.

<span class="caption">3-2-jadval: Rustdagi Integer literallar</span>

| Raqamli harflar   | Misol         |
|-------------------|---------------|
| O'nlik            | `98_222`      |
| O'n oltilik       | `0xff`        |
| Sakkizlik         | `0o77`        |
| Ikkilik           | `0b1111_0000` |
| Bayt (faqat "u8") | `b'A'`        |

Xo'sh, qaysi turdagi integer sonni ishlatishni qanday bilasiz? Agar ishonchingiz komil bo'lmasa, Rustning standart sozlamalari odatda boshlash uchun yaxshi joylardir: integer son turlari standart bo'yicha `i32` dir. `isize` yoki `usize` dan foydalanadigan asosiy holat to'plamning bir turini indekslashdir.

> ##### Integer Overflow
>
> Let’s say you have a variable of type `u8` that can hold values between 0 and
> 255. If you try to change the variable to a value outside that range, such as
> 256, *integer overflow* will occur, which can result in one of two behaviors.
> When you’re compiling in debug mode, Rust includes checks for integer overflow
> that cause your program to *panic* at runtime if this behavior occurs. Rust
> uses the term *panicking* when a program exits with an error; we’ll discuss
> panics in more depth in the [“Unrecoverable Errors with
> `panic!`”][unrecoverable-errors-with-panic]<!-- ignore --> section in Chapter
> 9.
>
> When you’re compiling in release mode with the `--release` flag, Rust does
> *not* include checks for integer overflow that cause panics. Instead, if
> overflow occurs, Rust performs *two’s complement wrapping*. In short, values
> greater than the maximum value the type can hold “wrap around” to the minimum
> of the values the type can hold. In the case of a `u8`, the value 256 becomes
> 0, the value 257 becomes 1, and so on. The program won’t panic, but the
> variable will have a value that probably isn’t what you were expecting it to
> have. Relying on integer overflow’s wrapping behavior is considered an error.
>
> To explicitly handle the possibility of overflow, you can use these families
> of methods provided by the standard library for primitive numeric types:
>
> * Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`.
> * Return the `None` value if there is overflow with the `checked_*` methods.
> * Return the value and a boolean indicating whether there was overflow with
>   the `overflowing_*` methods.
> * Saturate at the value’s minimum or maximum values with the `saturating_*`
>   methods.

#### Floating-Point Types

Rust also has two primitive types for *floating-point numbers*, which are
numbers with decimal points. Rust’s floating-point types are `f32` and `f64`,
which are 32 bits and 64 bits in size, respectively. The default type is `f64`
because on modern CPUs, it’s roughly the same speed as `f32` but is capable of
more precision. All floating-point types are signed.

Here’s an example that shows floating-point numbers in action:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

Floating-point numbers are represented according to the IEEE-754 standard. The
`f32` type is a single-precision float, and `f64` has double precision.

#### Numeric Operations

Rust supports the basic mathematical operations you’d expect for all the number
types: addition, subtraction, multiplication, division, and remainder. Integer
division truncates toward zero to the nearest integer. The following code shows
how you’d use each numeric operation in a `let` statement:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

Each expression in these statements uses a mathematical operator and evaluates
to a single value, which is then bound to a variable. [Appendix
B][appendix_b]<!-- ignore --> contains a list of all operators that Rust
provides.

#### The Boolean Type

As in most other programming languages, a Boolean type in Rust has two possible
values: `true` and `false`. Booleans are one byte in size. The Boolean type in
Rust is specified using `bool`. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

The main way to use Boolean values is through conditionals, such as an `if`
expression. We’ll cover how `if` expressions work in Rust in the [“Control
Flow”][control-flow]<!-- ignore --> section.

#### The Character Type

Rust’s `char` type is the language’s most primitive alphabetic type. Here are
some examples of declaring `char` values:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

Note that we specify `char` literals with single quotes, as opposed to string
literals, which use double quotes. Rust’s `char` type is four bytes in size and
represents a Unicode Scalar Value, which means it can represent a lot more than
just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji;
and zero-width spaces are all valid `char` values in Rust. Unicode Scalar
Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive.
However, a “character” isn’t really a concept in Unicode, so your human
intuition for what a “character” is may not match up with what a `char` is in
Rust. We’ll discuss this topic in detail in [“Storing UTF-8 Encoded Text with
Strings”][strings]<!-- ignore --> in Chapter 8.

### Compound Types

*Compound types* can group multiple values into one type. Rust has two
primitive compound types: tuples and arrays.

#### The Tuple Type

A *tuple* is a general way of grouping together a number of values with a
variety of types into one compound type. Tuples have a fixed length: once
declared, they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of the
different values in the tuple don’t have to be the same. We’ve added optional
type annotations in this example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

The variable `tup` binds to the entire tuple because a tuple is considered a
single compound element. To get the individual values out of a tuple, we can
use pattern matching to destructure a tuple value, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

This program first creates a tuple and binds it to the variable `tup`. It then
uses a pattern with `let` to take `tup` and turn it into three separate
variables, `x`, `y`, and `z`. This is called *destructuring* because it breaks
the single tuple into three parts. Finally, the program prints the value of
`y`, which is `6.4`.

We can also access a tuple element directly by using a period (`.`) followed by
the index of the value we want to access. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

This program creates the tuple `x` and then accesses each element of the tuple
using their respective indices. As with most programming languages, the first
index in a tuple is 0.

The tuple without any values has a special name, *unit*. This value and its
corresponding type are both written `()` and represent an empty value or an
empty return type. Expressions implicitly return the unit value if they don’t
return any other value.

#### The Array Type

Another way to have a collection of multiple values is with an *array*. Unlike
a tuple, every element of an array must have the same type. Unlike arrays in
some other languages, arrays in Rust have a fixed length.

We write the values in an array as a comma-separated list inside square
brackets:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Arrays are useful when you want your data allocated on the stack rather than
the heap (we will discuss the stack and the heap more in [Chapter
4][stack-and-heap]<!-- ignore -->) or when you want to ensure you always have a
fixed number of elements. An array isn’t as flexible as the vector type,
though. A *vector* is a similar collection type provided by the standard
library that *is* allowed to grow or shrink in size. If you’re unsure whether
to use an array or a vector, chances are you should use a vector. [Chapter
8][vectors]<!-- ignore --> discusses vectors in more detail.

However, arrays are more useful when you know the number of elements will not
need to change. For example, if you were using the names of the month in a
program, you would probably use an array rather than a vector because you know
it will always contain 12 elements:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

You write an array’s type using square brackets with the type of each element,
a semicolon, and then the number of elements in the array, like so:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, `i32` is the type of each element. After the semicolon, the number `5`
indicates the array contains five elements.

You can also initialize an array to contain the same value for each element by
specifying the initial value, followed by a semicolon, and then the length of
the array in square brackets, as shown here:

```rust
let a = [3; 5];
```

The array named `a` will contain `5` elements that will all be set to the value
`3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3];` but in a
more concise way.

##### Accessing Array Elements

An array is a single chunk of memory of a known, fixed size that can be
allocated on the stack. You can access elements of an array using indexing,
like this:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

In this example, the variable named `first` will get the value `1` because that
is the value at index `[0]` in the array. The variable named `second` will get
the value `2` from index `[1]` in the array.

##### Invalid Array Element Access

Let’s see what happens if you try to access an element of an array that is past
the end of the array. Say you run this code, similar to the guessing game in
Chapter 2, to get an array index from the user:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

This code compiles successfully. If you run this code using `cargo run` and
enter `0`, `1`, `2`, `3`, or `4`, the program will print out the corresponding
value at that index in the array. If you instead enter a number past the end of
the array, such as `10`, you’ll see output like this:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

The program resulted in a *runtime* error at the point of using an invalid
value in the indexing operation. The program exited with an error message and
didn’t execute the final `println!` statement. When you attempt to access an
element using indexing, Rust will check that the index you’ve specified is less
than the array length. If the index is greater than or equal to the length,
Rust will panic. This check has to happen at runtime, especially in this case,
because the compiler can’t possibly know what value a user will enter when they
run the code later.

This is an example of Rust’s memory safety principles in action. In many
low-level languages, this kind of check is not done, and when you provide an
incorrect index, invalid memory can be accessed. Rust protects you against this
kind of error by immediately exiting instead of allowing the memory access and
continuing. Chapter 9 discusses more of Rust’s error handling and how you can
write readable, safe code that neither panics nor allows invalid memory access.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[twos-complement]: https://en.wikipedia.org/wiki/Two%27s_complement
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[appendix_b]: appendix-02-operators.md
