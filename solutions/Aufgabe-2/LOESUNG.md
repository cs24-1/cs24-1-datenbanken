# Lösung Aufgabe 2

## 1.

1. Erstelle die Tabelle `customer` mit den geforderten Eigenschaften aus dem ERM:
    - nutze für `id` die Eigenschaften `AUTO_INCREMENT`, `PRIMARY KEY`
    - nutze für `birthday` den Datentyp `DATETIME`.

```sql
CREATE TABLE customer (
    id INT AUTO_INCREMENT PRIMARY KEY,
    surname VARCHAR(50) NOT NULL,
    forename VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    birthday DATETIME NOT NULL
);
```

## 2.

2. Erstelle ähnlich dazu die Tabelle `employee`.

```sql
CREATE TABLE employee (
    id INT AUTO_INCREMENT PRIMARY KEY,
    surname VARCHAR(50) NOT NULL,
    forename VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

## 3.

3. Erstelle die Tabelle `car` mit den geforderten Eigenschaften aus dem ERM mit den zusätzlichen Attributen:
    - `managed_by_employee_id` : Fremdschlüssel zum Mitarbeiter, der das Auto verwaltet
    - `bought_by_customer_id` : Fremdschlüssel zum Kunden, der das Auto gekauft hat.

```sql
CREATE TABLE car (
    id INT AUTO_INCREMENT PRIMARY KEY,
    brand VARCHAR(50) NOT NULL,
    color VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    managed_by_employee_id INT,
    bought_by_customer_id INT,
    FOREIGN KEY (managed_by_employee_id) REFERENCES employee(id) ON DELETE RESTRICT,
    FOREIGN KEY (bought_by_customer_id) REFERENCES customer(id) ON DELETE RESTRICT
);
```

## 4.

4. Füge nun einige Datensätze ein.

Die Daten findest du in [COPY.md](COPY.md).

### Customer

```sql
INSERT INTO customer (
    surname, forename, email, birthday
) VALUES
    ('Müller', 'Max', 'max.mueller@mail.com', '1995-06-15'),
    ('Schmidt', 'Anna', 'anna.schmidt@company.com', '2000-01-10'),
    ('Krause', 'Peter', 'peter.krause@mail.com', '1992-09-25'),
    ('Becker', 'Julia', 'julia.becker@provider.com', '1985-12-05'),
    ('Fischer', 'Tom', 'tom.fischer@provider.com', '2006-03-30');
```

### Employee

```sql
INSERT INTO employee (
    surname, forename, email
) VALUES
    ('Meier', 'Klaus', 'klaus.meier@provider.com'),
    ('Schmidt', 'Sara', 'sara.schmidt@company.com'),
    ('Weber', 'Paul', 'paul.weber@provider.com'),
    ('Kunze', 'Laura', 'laura.kunze@mail.com'),
    ('Kramer', 'Heinz', 'heinz.kramer@mail.com');
```

### Car

```sql
INSERT INTO car (
    brand, color, price, managed_by_employee_id, bought_by_customer_id
) VALUES
    ('Peugeot', 'red', 30000.00, 1, 1),
    ('Audi', 'blue', 35000.00, 2, 2),
    ('Peugeot', 'black', 40000.00, 3, 1),
    ('Volkswagen', 'white', 25000.00, 4, NULL),
    ('Audi', 'grey', 27000.00, 3, NULL);
```
