```mermaid
erDiagram
    Customer ||--o{ Order : "places (0 to N)"
    Album ||--|{ Song : "contains (1 to N)"
    Employee ||--o| CompanyCar : "assigned (0 or 1)"
    Country ||--|| Flag : "has (1 only)"
    
    %% ||  = Exactly One (Mandatory)
    %% o|  = Zero or One (Optional)
    %% |{  = One or Many (Mandatory)
    %% o{  = Zero or Many (Optional)
```

<br>

`order` -> `Customer`
- An order has to be assigned to exactly one `||` customer.
    - (There cant be an order without a customer)
    - (an order cannot be assigned to multiple customers)

<br>

`Customer` -> `order`
- A customer can have no orders/one order/many orders `0{` customer.

<br>
<br>
<br>
<br>

`Song` → `Album`
- Each Song must belong to exactly one Album `||`.

<br>

`Album` → `Song`
An Album must contain at least one Song, but can have many `|{`.
- An empty album with zero songs is not valid (business rule).
- Most albums contain multiple songs.





