
|**Syntax**|**Description**|
|---|---|
|`try: ... except: ...`|Basic exception handling|
|`except ValueError as e:`|নির্দিষ্ট exception ধরা|
|`except (TypeError, ValueError):`|একাধিক exception একসাথে ধরা|
|`else:`|Exception না হলে রান হবে|
|`finally:`|Exception হোক বা না হোক সবসময় রান হবে|
|`raise ValueError("msg")`|নিজে exception তোলা|
|`class MyError(Exception): pass`|Custom exception class|

## মূল কথা

- Data loading/preprocessing-এ ফাইল না পাওয়া (`FileNotFoundError`), ভুল টাইপ (`TypeError`), missing value নিয়ে ভুল হিসাব (`ValueError`) — এসব প্রচুর হয়, তাই `try-except` জানা জরুরি।
- `except:` (কোনো টাইপ না দিয়ে) সব ধরনের exception ধরে ফেলে — এটা debugging-এ ভুল লুকিয়ে ফেলতে পারে, তাই যতটা সম্ভব নির্দিষ্ট exception type ধরাই ভালো অভ্যাস।