
# 📘 Python Concepts Explained in Roman Urdu (Detail + Examples)

Yeh file un sab Python ke concepts ko detail mein samjhati hai jo humne discuss kiye, saath mein **Roman Urdu explanations** aur real code examples diye gaye hain.

---

## ✅ 1. Generics in Python Typing

### 🔸 Roman Urdu Mein:

`Generics` ka matlab hota hai ke aap koi aisi function ya class banayein jo **multiple types ke sath kaam kar sake**.

Yani agar aap ek list handle kar rahe ho, lekin wo kabhi `int`, kabhi `str`, kabhi `float` ho sakti hai — to har baar alag function likhne ke bajaye ek hi generic function likh dete hain.

Aap `TypeVar` ka use karte hain taake ek flexible aur reusable type define kar sakein.

### 🔹 Code Example:

```python
from typing import TypeVar, List

T = TypeVar('T')  # T can be int, str, etc.

def get_first_item(items: List[T]) -> T:
    return items[0]
```

### ❓ Sawal: Hum `Any` kyu nahi use karte?

- `Any` ka matlab hota hai *kuch bhi chalega* — koi type checking nahi.
- Lekin `Generic` se Python ko pata hota hai ke agar input `str` list hai, to output bhi `str` hi hoga.
- Is se static type checkers (jaise mypy) better errors pakar sakte hain.

**🧠 Generic: Type safe hota hai, Any: Wildcard hota hai.**

# Sequence aur Generic Type Hinting - Roman Urdu Main

Is file mein hum `Sequence`, `Tuple`, `List` aur `Generic` type hinting Python mein kis tarah kaam karti hai wo Roman Urdu mein detail se samjhayenge.

## 🔹 TypeVar aur Generic ka istemal

Agar aap function ya class likh rahe hain jo kisi bhi type ke sath kaam kar sakta hai (for example: `int`, `str`, `float`, `list`, `tuple`), tu aap `TypeVar` aur `Generic` ka use karte hain.

```python
from typing import TypeVar, Sequence

T = TypeVar('T')

def first_item(seq: Sequence[T]) -> T:
    return seq[0]
```

### 💡 Is function ka kya faida hai?
Yeh function `Sequence[T]` accept karta hai, jahan `T` koi bhi type ho sakti hai jaise `int`, `str`, ya `float`. Aur return bhi wahi type karega.

Example:
```python
print(first_item([1, 2, 3]))           # Output: 1
print(first_item(("a", "b", "c")))     # Output: "a"
```

## 🔹 Sequence kya hota hai?

Python mein `Sequence` ek abstract type hai jo batata hai ke object indexed aur iterable ho (usmein index se value access ho sakti hai). `list`, `tuple`, `str` sab `Sequence` hain.

```python
from typing import Sequence

def print_items(seq: Sequence[str]) -> None:
    for item in seq:
        print(item)
```

### ✅ Accept karega:
- `["apple", "banana"]` (list)
- `("apple", "banana")` (tuple)

### ❌ Accept nahi karega:
- `{ "apple", "banana" }` (set)
- `{ "a": 1 }` (dict)

## 🔹 List aur Tuple ka farq

| Feature        | List                       | Tuple                     |
|----------------|----------------------------|---------------------------|
| Mutable        | Haan (change ho sakta hai) | Nahi (change nahi hota)   |
| Syntax         | `[1, 2, 3]`                | `(1, 2, 3)`               |
| Use Case       | Jab change karna ho        | Jab fix items ho          |
| Performance    | Dheere                    | Thoda fast                |

## 🔹 Kyun `Sequence[str]` mein sirf list aur tuple chalte hain?
Kyun ke `Sequence` sirf wo types accept karta hai jo indexed aur iterable hoti hain. Sets aur dicts unordered hote hain, isliye wo `Sequence` nahi hain.

## Summary

- `Generic` se hum flexible function bana sakte hain.
- `Sequence` ek abstract type hai jo list, tuple aur string ko represent karta hai.
- `TypeVar` se hum return type automatically match kar sakte hain with input.


---

## ✅ 2. `Callable` (Typing Hint)

### 🔸 Roman Urdu Mein:

`Callable` ka matlab hota hai koi aisi cheez (function ya object) jisko `()` laga ke call kiya ja sakta hai.

Type hint ke tor par jab aap `Callable[[InputType], ReturnType]` likhte hain to aap define kar rahe hote hain ke:

> Yeh ek function hai jo `InputType` lega aur `ReturnType` dega.

### 🔹 Code Example:

```python
from typing import Callable

def run_func(fn: Callable[[int], int], value: int) -> int:
    return fn(value)

def double(x: int) -> int:
    return x * 2

print(run_func(double, 5))  # Output: 10
```

---

## ✅ 3. `__call__` Method (Magic Method)

### 🔸 Roman Urdu Mein:

Agar aap chahte ho ke **kisi class ka object function ki tarah kaam kare**, to aap usme `__call__()` method define karte ho.

Phir aap us object ko `()` ke sath call kar sakte ho.

### 🔹 Code Example:

```python
class Adder:
    def __call__(self, a, b):
        return a + b

sum_obj = Adder()
print(sum_obj(2, 3))  # Output: 5
```

### 🤔 Note:

- `__call__` wale object bhi `callable(obj)` ke test mein `True` dete hain.
- Ye pattern decorators aur function wrappers mein useful hota hai.

---

## ✅ 4. `field(default_factory=list)` in Dataclasses

### 🔸 Roman Urdu Mein (Detail Se):

Jab aap Python ki `@dataclass` use karte hain aur kisi attribute ko list/dict default dena chahte ho, to **direct `=[]` dena galat approach hoti hai**.

### ❗ Problem with `items: List[str] = []`:

Is se kya hota hai:

- Python **ek hi list sab objects ke liye share kar deta hai**.
- Yani agar ek object list mein item add kare, to wo doosre object mein bhi dikhayi degi.

### 🔹 Solution: Use `default_factory=list`

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Cart:
    items: List[str] = field(default_factory=list)

cart1 = Cart()
cart2 = Cart()

cart1.items.append("Apple")

print(cart1.items)  # ['Apple']
print(cart2.items)  # [] ✅ Alag list
```

### 🧠 Roman Urdu Mein Summary:

- `default_factory=list` isliye use karte hain taake **har object ko nayi list mile**, shared na ho.
- Ye memory efficient bhi hai — jab tak object banta nahi, tab tak list banegi nahi (lazy evaluation).

---

## ✅ 5. Difference: `__call__` vs `Callable`

| Feature         | `__call__`                           | `Callable` (Typing)                     |
|-----------------|--------------------------------------|-----------------------------------------|
| Kya hai?         | Ek method jo object ko callable banata hai | Ek type hint hai                       |
| Kahan use hota?   | Class ke andar                      | Function parameters ya annotations mein |
| Runtime ya static? | Runtime behavior deta hai           | Static checking ke liye hota hai        |
| Example          | `obj()` agar `__call__` defined ho    | `fn: Callable[[int], str]`              |

---

## ✅ Summary Table (Quick Recap)

| Concept | Matlab (Roman Urdu) | Use Kahan Hota Hai |
|--------|----------------------|--------------------|
| `Generics` | Flexible type hinting | Reusable functions/classes |
| `Callable` | Type jo call ho sake | Function parameters ke liye |
| `__call__` | Object ko `()` se call karna | Functional objects banane ke liye |
| `default_factory` | Har object ko nayi list dena | Dataclasses mein mutable defaults ke liye |

---

## 🔚 End Note

Yeh file aapko Python ke advanced concepts ko easily samajhne mein madad karegi. Har topic ke sath real-life use-case diya gaya hai.

Agar aapko aur concepts add karwane hain (jaise decorators, context managers, etc) to zaroor batayen.
