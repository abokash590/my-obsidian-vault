পাইথন **dynamically typed** — variable declare করার সময় type বলে দিতে হয় না, value দেখেই পাইথন type বুঝে নেয়।

|**Syntax**|**Description**|
|---|---|
|`x = 5`|Integer (`int`)|
|`x = 5.0`|Float|
|`x = "hello"` / `'hello'`|String (`str`)|
|`x = True` / `False`|Boolean (`bool`)|
|`x = None`|Null value (`NoneType`)|
|`x = 3 + 2j`|Complex number|
|`a, b = 1, 2`|একসাথে multiple variable assign|
|`a, b = b, a`|Temp variable ছাড়া swap|
|`a = b = c = 0`|একই value দিয়ে একাধিক variable assign|
|`type(x)`|`x`-এর data type রিটার্ন করে|
|`int(x)` / `float(x)` / `str(x)` / `bool(x)`|Type casting|
|`isinstance(x, int)`|`x` ওই type এর কিনা চেক করে|

## মূল কথা

- Python-এ সব কিছুই object — এমনকি `int`, `str` ও।
- `None` মানে "কিছু নেই" — C++ এর `nullptr`-এর মতো, কিন্তু এটা নিজেই একটা type।
- Type casting করার সময় invalid conversion হলে (যেমন `int("abc")`) `ValueError` আসবে।