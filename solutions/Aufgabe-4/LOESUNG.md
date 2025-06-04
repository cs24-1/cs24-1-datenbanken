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

> Zunächst müssen wir die Grenzen rausfinden. Wir haben 2025, also suchen wir Kunden, die zwischen 1990 und 1995 geboren wurden.

```sql
SELECT forename, email
FROM customer
WHERE birthday BETWEEN '1990-01-01' AND '1995-12-31';
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

### ID von Max Müller

```sql
SELECT id
FROM customer
WHERE surname = 'Müller' AND forename = 'Max';
```

### Mitarbeiter-IDs von Autos, die Max Müller gekauft hat

```sql
SELECT managed_by_employee_id
FROM car
WHERE bought_by_customer_id = <max_müller_id>;
```

### Mitarbeiternamen

```sql
SELECT surname, forename
FROM employee
WHERE id IN (<id1>, <id2>, ...);
```

## 6.

6. Welche Mitarbeiter haben noch unverkaufte Autos?

### Mitarbeiter-IDs von Autos, die nicht verkauft wurden

```sql
SELECT managed_by_employee_id
FROM car
WHERE bought_by_customer_id IS NULL;
```

### Mitarbeiternamen

```sql
SELECT surname, forename
FROM employee
WHERE id IN (<id1>, <id2>, ...);
```

## 7.

7. Jeder Mitarbeiter erhält 15% Provision pro verkauftes Auto.
    - Wie viel Provision erhielte Paul Weber, wenn er alle seine Autos verkauft?
    - Führe die Berechnung in SQL durch.

### Mitarbeiter-ID von Paul Weber

```sql
SELECT id
FROM employee
WHERE surname = 'Weber' AND forename = 'Paul';
```

### Wert der Autos, die Paul Weber verwaltet

```sql
SELECT SUM(price) * 0.15 AS commission
FROM car
WHERE managed_by_employee_id = <paul_weber_id>;
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
