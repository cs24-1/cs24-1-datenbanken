# Lösung Aufgabe 4

## 1.

1. Wie viele Mitarbeiter sind aktuell im Unternehmen angestellt?

```sql
SELECT COUNT(*)
FROM employee;
```

## 2.

2. Wir möchten mit dem Vornamen personalisierte Werbe-E-Mails verschicken.
    1. Welche Namen und E-Mail-Adressen brauchen wir dafür?
    2. Welche dieser Kunden sind zwischen 30 und 35 Jahre alt?

```sql
SELECT forename, email
FROM customer
WHERE birthday BETWEEN DATE_SUB(CURDATE(), INTERVAL 35 YEAR) AND DATE_SUB(CURDATE(), INTERVAL 30 YEAR);
```

## 3.

3. Welche verschiedenen Automarken werden angeboten?
    1. Gib die Liste geordnet zurück.
    2. Wie viele Autos gibt es von jeder Marke?

```sql
-- Mit DISTINCT
SELECT DISTINCT brand
FROM car
ORDER BY brand;

-- Mit GROUP BY
SELECT brand, COUNT(*) AS car_count
FROM car
GROUP BY brand
ORDER BY brand;
```

## 4.

4. Was ist der Gesamtwert aller Autos im Autohaus?

```sql
SELECT SUM(price) AS total_value
FROM car;
```

## 5.

5. Welche Mitarbeiter haben Autos an Max Müller verkauft?

```sql
SELECT e.surname, e.forename
FROM employee e
JOIN car c ON e.id = c.managed_by_employee_id
JOIN customer cu ON c.bought_by_customer_id = cu.id
WHERE cu.surname = 'Müller' AND cu.forename = 'Max';
```

## 6.

1. Welche Mitarbeiter haben noch unverkaufte Autos?

```sql
SELECT e.surname, e.forename
FROM employee e
JOIN car c ON e.id = c.managed_by_employee_id
WHERE c.bought_by_customer_id IS NULL;
```

## 7.

7. Jeder Mitarbeiter erhält 15% Provision pro verkauftes Auto.
    - Wie viel Provision erhielte Paul Weber, wenn er alle seine Autos verkauft?
    - Führe die Berechnung in SQL durch.

```sql
SELECT SUM(c.price) * 0.15 AS commission
FROM employee e
JOIN car c ON e.id = c.managed_by_employee_id
WHERE e.surname = 'Weber' AND e.forename = 'Paul';
```

## 8.

8. Es gab einen Hack beim E-Mail-Provider `mail.com`. Welche E-Mail-Adressen dieses Providers stehen in unserem System?

```sql
SELECT email
FROM (
    SELECT email FROM customer
    UNION
    SELECT email FROM employee
) AS all_emails
WHERE email LIKE '%@mail.com';
```
