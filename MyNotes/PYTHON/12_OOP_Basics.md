
|**Syntax**|**Description**|
|---|---|
|`class MyClass:`|Class define করা|
|`def __init__(self, ...):`|Constructor|
|`self.attr = val`|Instance attribute|
|`obj = MyClass(...)`|Object তৈরি|
|`def method(self, ...):`|Instance method (সবসময় প্রথম parameter `self`)|
|`class Child(Parent):`|Inheritance|
|`super().__init__(...)`|Parent class-এর constructor কল করা|
|`def __str__(self): return "..."`|`print(obj)` করলে যা দেখাবে|
|`def __eq__(self, other):`|`==` operator override|
|`def __len__(self):`|`len(obj)` override|
|`@staticmethod`|Instance ছাড়াই কল করা যায় এমন method|
|`@classmethod`|প্রথম argument হিসেবে class (`cls`) পায়|

## মূল কথা

- Python-এ পুরো OOP জানা ML শুরুতে বাধ্যতামূলক না, কিন্তু scikit-learn/PyTorch-এর মতো library-র কোড পড়তে ও নিজের `Dataset`/`Model` class বানাতে বেসিক class-object-inheritance জানা লাগবেই।
- `self` মানে "নিজের instance"-কে বোঝানো — C++-এর `this` pointer-এর মতোই, শুধু explicit ভাবে প্রতিটা method-এ প্রথম parameter হিসেবে লিখতে হয়।