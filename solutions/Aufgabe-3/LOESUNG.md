# Lösung Aufgabe 3

## 1.

1. Julia Becker hat den weißen Volkswagen gekauft.
    - Was muss gemacht werden?

> Zunächst benötigen wir die ID von Julia Becker, um diesen am Auto einzutragen.

```sql
SELECT id
FROM customer
WHERE surname = 'Becker'
    AND forename = 'Julia';
```

> Nun können wir das Auto aktualisieren, indem wir die `bought_by_customer_id` auf die ID von Julia Becker setzen.

```sql
UPDATE car
SET bought_by_customer_id = 5
WHERE
    brand = 'Volkswagen'
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

SELECT id
FROM customer
WHERE surname = 'Dummy' AND forename = 'Kunde';
```

### Autos von Anna Schmidt aktualisieren

```sql
SELECT id
FROM customer
WHERE surname = 'Schmidt' AND forename = 'Anna';

UPDATE car
SET bought_by_customer_id = <dummy_customer_id>
WHERE bought_by_customer_id = <anna_schmidt_id>;
```

### Anna Schmidt löschen

```sql
DELETE FROM customer
WHERE surname = 'Schmidt' AND forename = 'Anna';
```
