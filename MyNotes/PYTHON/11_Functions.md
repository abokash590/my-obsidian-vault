
|**Syntax**|**Description**|
|---|---|
|`def f(a, b): return a + b`|সাধারণ function|
|`def f(a, b=10):`|Default argument|
|`def f(*args):`|Variable positional arguments (tuple হিসেবে আসে)|
|`def f(**kwargs):`|Variable keyword arguments (dict হিসেবে আসে)|
|`f(*v)`|List/tuple unpack করে argument হিসেবে পাস করা|
|`f(**d)`|Dict unpack করে keyword argument হিসেবে পাস করা|
|`lambda x, y: x + y`|Anonymous (একলাইনের) function|
|`sorted(v, key=lambda x: -x)`|Lambda-কে sort-এর key হিসেবে ব্যবহার|
|`def f() -> int:`|Return type hint (optional, শুধু readability-র জন্য)|
|`def f(x: int, y: int) -> int:`|Parameter type hint|

## মূল কথা

- Function-এর ভিতরে define করা variable local — বাইরে থেকে access করা যায় না, `global` keyword ছাড়া বাইরের variable পরিবর্তনও করা যায় না।
- `*args` আর `**kwargs` একসাথে ব্যবহার করা যায়: `def f(a, *args, **kwargs):` — এটা flexible API/wrapper function লেখার সময় খুব কাজে লাগে।
- Type hint (`x: int`) পাইথন enforce করে না, শুধু IDE/readability-র জন্য সাহায্য করে — ML কোডে (বিশেষত library-তে) এটা খুব কমন।