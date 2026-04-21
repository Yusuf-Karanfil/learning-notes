# OOP — Nesne Tabanlı Programlama

## Dört Temel Kavram

| Kavram | Tanım |
|--------|-------|
| **Encapsulation** | Veri ve davranışı bir arada tutmak |
| **Inheritance** | Bir classtan başka bir class türetmek |
| **Polymorphism** | Aynı metodun farklı classlarda farklı davranması |
| **Abstraction** | Gereksiz detayı gizlemek |

---

## 1. Encapsulation

"Phone" ile ilgili her şey tek bir yerde.

```python
class Phone:
    def __init__(self, brand, model, price, stock):
        self.brand = brand
        self.model = model
        self.price = price
        self.stock = stock

    def describe(self):
        return f"{self.brand} {self.model} - {self.price} TL"

    def is_available(self):
        return self.stock > 0

tel1 = Phone("Apple", "iPhone 15", 45000, 10)
```

---

## 2. Inheritance

Bir classtan başka bir class türetmek.

**Senaryo:** Telefoncu zamanla aksesuar ve tablet satmaya başladı. Her ürünün markası, fiyatı, stoğu olmalı. Ama tabletin ayrıyetten ekran boyutu, aksesuarın uyumlu olduğu model var.

```python
class Product:
    def __init__(self, brand, price, stock):
        self.brand = brand
        self.price = price
        self.stock = stock

    def is_available(self):
        return self.stock > 0

    def describe(self):
        return f"{self.brand} - {self.price} TL"


class Phone(Product):
    def __init__(self, brand, price, stock, storage):
        super().__init__(brand, price, stock)
        self.storage = storage

    def describe(self):
        return f"{self.brand} {self.storage} GB - {self.price} TL"


class Tablet(Product):
    def __init__(self, brand, price, stock, screen_size):
        super().__init__(brand, price, stock)
        self.screen_size = screen_size
```

---

## 3. Polymorphism

Aynı metodun farklı classlarda farklı davranması.

```python
class Product:
    def discounted_price(self):
        return self.price


class Phone(Product):
    def discounted_price(self):
        return self.price * 0.90  # %10 indirim


class Tablet(Product):
    def discounted_price(self):
        return self.price * 0.85  # %15 indirim


products = [Phone(...), Tablet(...), Phone(...)]

for product in products:
    print(product.discounted_price())
```

Her nesne aynı metodu çağırıyor ama her biri kendi kuralına göre davranıyor.

---

## 4. Abstraction

Gereksiz detayı gizlemek, sadece ihtiyaç anında göster.

**Senaryo:** 3 farklı ödeme yöntemi var. Sen sadece ödeme yapmak istiyorsun. Ödeme yönteminin şeklini değil — sadece "öde" demek yeterli.

```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    @abstractmethod
    def pay(self, amount):
        pass


class CreditCard(PaymentMethod):
    def pay(self, amount):
        print(f"Kredi kartından {amount} TL çekildi.")


class BankTransfer(PaymentMethod):
    def pay(self, amount):
        print(f"Banka havalesiyle {amount} TL gönderildi.")


class CashOnDelivery(PaymentMethod):
    def pay(self, amount):
        print(f"Kapıda {amount} TL ödendi.")


def checkout(payment: PaymentMethod, amount):
    payment.pay(amount)


checkout(CreditCard(), 45000)
```

`PaymentMethod` direkt kullanılamaz — sadece türetilir. Her alt class `pay()` metodunu kendi yöntemiyle uygulamak zorunda.
