# Lösung Aufgabe 3

## 1.

1. Julia Becker hat den weißen Volkswagen gekauft.
    - Was muss gemacht werden?

> Wir können das als eine Query machen.

```sql
UPDATE car
SET bought_by_customer_id = (
    SELECT id
    FROM customer
    WHERE surname = 'Becker' AND forename = 'Julia'
)
WHERE brand = 'Volkswagen'
  AND color = 'white';
```

## 2.

2. Anna Schmidt möchte, dass all ihre Daten gelöscht werden.
    - Wie kann das gemacht werden?
    - War unsere Entscheidung für `ON DELETE` bei der Verknüpfung richtig?
    - Was ist nun die beste Alternative?

> Die Daten von Anna Schmidt können gelöscht werden, indem wir den entsprechenden Datensatz in der `customer`-Tabelle löschen.

> Wir wollen sicherstellen, dass gekaufte Autos nich gelöscht werden, wenn der Kunde gelöscht wird. Daher haben wir `ON DELETE RESTRICT` verwendet, was bedeutet, dass ein Kunde nicht gelöscht werden kann, wenn er Autos gekauft hat.

> Eine Alternative dazu ist, einen Dummy-Kunden zu erstellen, der die gekauften Autos verwaltet, anstatt sie zu löschen. Das würde bedeuten, dass wir die `bought_by_customer_id` auf diesen Dummy-Kunden setzen.

### Dummy-Kunden erstellen

```sql
INSERT INTO customer (surname, forename, email, birthday)
VALUES ('Dummy', 'Kunde', 'dummy.kunde@example.com', '2000-01-01');
```

### Autos von Anna Schmidt aktualisieren

```sql
UPDATE car
SET bought_by_customer_id = (
    SELECT id FROM customer WHERE surname = 'Dummy' AND forename = 'Kunde'
)
WHERE bought_by_customer_id = (
    SELECT id FROM customer WHERE surname = 'Schmidt' AND forename = 'Anna'
);
```

### Anna Schmidt löschen

```sql
DELETE FROM customer
WHERE surname = 'Schmidt' AND forename = 'Anna';
```
