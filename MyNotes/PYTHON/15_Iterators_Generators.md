
|**Syntax**|**Description**|
|---|---|
|`def gen(): yield x`|Generator function (lazy evaluation)|
|`next(iterator)`|পরবর্তী value নেওয়া|
|`iter(v)`|Iterable থেকে iterator বানানো|
|`(x for x in v)`|Generator expression|
|`yield from other_gen()`|আরেক generator থেকে yield করা|

## মূল কথা

- Generator পুরো output একসাথে মেমরিতে না রেখে, একটা একটা করে value দেয় — বড় dataset (যেমন লক্ষ লক্ষ row) batch-wise process করার সময় (ML training loop-এ খুব কমন প্যাটার্ন) এটা memory বাঁচায়।
- List আর generator দেখতে একইরকম কাজ করে মনে হলেও, generator শুধু একবারই iterate করা যায় — শেষ হয়ে গেলে আবার শুরু থেকে চালানো যায় না, নতুন করে বানাতে হয়।