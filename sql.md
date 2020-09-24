# SQL (MySQL) - Powtórka

## 1. Podstawy:

Logowanie do bazy danych:
```
mysql -u USER -p PASSWORD
mysql -u root

MariaDB [(none)]>
```

Sprawdzenie wersji bazy danych:
```sql
SHOW VARIABLES LIKE "%version%";
\s
```

Wylistowanie baz danych:
```sql
SHOW DATABASES;
```

Tworzenie bazy danych z polskimi znakami:
```sql
CREATE DATABASE dbname;
ALTER DATABASE dbname DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```
```sql
CREATE DATABASE dbname DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

Wybranie bazy danych:
```sql
USE dbname;
```

Sprawdzenie szczegółów utworzonej bazy:
```sql
SHOW CREATE DATABASE dbname;
```

Usuwanie bazy danych:
```sql
DROP DATABASE dbname;
```

## 2. Tworzenie użytkowników i system uprawnień:

Wylistowanie użytkowników:
```sql
SELECT User, Host, Password FROM mysql.user;
```

Tworzenie użytkownika poleceniem `CREATE`:
```sql
CREATE USER "user"@"localhost" IDENTIFIED BY "password";
```

Usuwanie użytkownika:
```sql
DROP USER "user"@"localhost";
```

Składnia polecenia `GRANT`:
```sql
GRANT
    priv_type [(column_list)]
      [, priv_type [(column_list)]] ...
    ON [object_type] priv_level
    TO user [auth_option] [, user [auth_option]] ...
    [REQUIRE {NONE | tls_option [[AND] tls_option] ...}]
    [WITH {GRANT OPTION | resource_option} ...]
```

Tworzenie użytkownika poleceniem `GRANT`:
```sql
GRANT ALL ON * TO "user"@"localhost" IDENTIFIED BY "password";
```

Tworzenie użytkownika poleceniem `CREATE` i dodanie uprawnień poleceniem `GRANT`:
```sql
CREATE USER "user"@"localhost" IDENTIFIED BY 'password';
GRANT ALL ON dbname.* TO "user"@"localhost";
```

Pokazanie uprawnień użytkownika:
```sql
SHOW GRANTS FOR "user"@"localhost";
SHOW GRANTS FOR CURRENT_USER;
```

Odebranie uprawnień użytkownikowi:
```sql
REVOKE ALL, GRANT OPTION ON * FROM "user"@"localhost";
REVOKE SELECT ON dbname.* FROM "user"@"localhost";
```

## 3. Tabele:
Tworzenie tabeli:
```sql
CREATE TABLE Persons (
    id INT,
    name VARCHAR(255),
    email VARCHAR(255),
);
```

Usuwanie tabeli:
```sql
DROP TABLE tableName;
```

Opis tabeli:
```sql
DESCRIBE tableName;
SHOW FIELDS FROM tableName;
```

Wylistowanie wszystkich tabel w bazie:
```sql
SHOW TABLES;
```

Szczegółowy opis tabeli w formie polecenia SQL:
```sql
SHOW CREATE TABLE tableName;
```

Tworzenie tabeli z kluczem głównym:
```sql
CREATE TABLE Persons (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255),
);
```

Tworzenie tabeli z wieloma kluczami głównymi:
```sql
CREATE TABLE OrderedBooks (
    id INT UNSIGNED NOT NULL,
    isbn CHAR(13) NOT NULL,
    quantity TINYINT UNSIGNED,
    PRIMARY KEY(id, isbn)
);
```

Zmiana nazwy tabeli:
```sql
RENAME TABLE tableName TO newTableName;
```

Stworznie kopii struktury tabeli:
```sql
CREATE TABLE newTable AS SELECT * FROM oldTable;
```

Stworzenie kopii całej tabeli:
```sql
CREATE TABLE newTable LIKE oldTable;
INSERT newTable SELECT * FROM oldTable;
```

## 4. Modyfikowanie tabel poleceniem `ALTER`:

Dodanie klucza głównego do tabeli:
```sql
ALTER TABLE tableName ADD CONSTRAINT constraintName PRIMARY KEY (id);
```

Porzucenie klucza głównego z tabeli:
```sql
ALTER TABLE tableName DROP PRIMARY KEY;
```

Dodanie nowej kolumny:
```sql
ALTER TABLE tableName ADD COLUMN columnName VARCHAR(50);
```

Dodanie nowej kolumny po istniejącej:
```sql
ALTER TABLE tableName ADD COLUMN columnName VARCHAR(50) AFTER existingColumn;
```

Dodanie nowej kolumny jako pierwszej:
```sql
ALTER TABLE tableName ADD COLUMN columnName VARCHAR(50) FIRST;
```

Dodanie nowej kolumny z kluczem głównym:
```sql
ALTER TABLE tableName ADD COLUMN columnName INT NOT NULL AUTO_INCREMENT PRIMARY KEY;
```

Zmiana nazwy kolumny, np. z kluczem głównym (po zmianie nazwy klucz zostaje):
```sql
ALTER TABLE tableName CHANGE "name" "newName" INT NOT NULL AUTO_INCREMENT;
```

Usunięcie kolumny z tabeli:
```sql
ALTER TABLE tableName DROP COLUMN "columnName";
```

Ustawienie wartości domyślnej:
```sql
ALTER TABLE tableName ALTER columnName SET DEFAULT "default";
```

Usunięcie wartości domyślnej:
```sql
ALTER TABLE tableName ALTER columnName DROP DEFAULT;
```

Ustawienie wartości unikalnych w kolumnie:
```sql
ALTER TABLE tableName ADD CONSTRAINT constraintName UNIQUE (columnName);
```

Usunięcie wartości unikalnej w kolumnie:
```sql
ALTER TABLE tableName DROP INDEX constraintName;
```

Dodanie do kolumny atrybutów (deklarujemy od nowa typ i atrybuty):
```sql
ALTER TABLE tableName MODIFY columnName VARCHAR(255) NOT NULL;
```

Usunięcie dodanych atrybutów (znowu deklaracja od nowa):
```sql
ALTER TABLE tableName MODIFY columnName VARCHAR(50);
```

## 5. Indeksowanie:

Dodanie pojedyńczego indeksu:
```sql
CREATE INDEX indexName ON tableName(column);
```

Dodanie złożonego indeksu:
```sql
CREATE INDEX indexName ON tableName(column1, column2, ...);
```

Dodanie indeksu przy tworzeniu tabeli:
```sql
CREATE TABLE tableName (
    id INT,
    name VARCHAR(50),
    INDEX indexName (id, name)
);
```

Pokazanie indeksów w danej tabeli:
```sql
SHOW INDEX FROM tableName;
```

Usunięcie indeksu:
```sql
DROP INDEX indexName on tableName;
```

## 6. Dodawanie rekordów do tabeli:

Ogólna składnia:
```sql
INSERT INTO tableName (column1, column2,...)
VALUES (value1, value2,....)
```

Sposób: dodawanie do wszystkich kolumn wartości (NULL wstawiamy do kolumny w której nic nie chcemy dac):
```sql
INSERT INTO tableName VALUES(NULL, "value2", "value3");
```

Sposób: wstawianie danych do konkretnych kolumn wartości:
```sql
INSERT INTO tableName (column1, column2, column3) VALUES ("value1", "value2", "value3");
```

Sposób: dodawanie danych poprzez przypisanie:
```sql
INSERT INTO tableName SET column1="value1", column2="value2", column3="value3";
```

Wstawianie kilku rekordów na raz:
```sql
INSERT INTO tableName VALUES
    (NULL, "value1", "value2"),
    (NULL, "value3", "value4"),
    (NULL, "value5", "value6");
```

## 7. Wyświetlanie rekordów z tabeli:

Wybranie wszystkich rekodów z tabeli:
```sql
SELECT * FROM tableName;
```

## 8. Modyfikowanie rekordów w tabeli:

Ogólna składnia polecenia `UPDATE`:
```sql
UPDATE tableName
    SET column1 = "value1", column2 = "value2", ...
    [WHERE condition];
```

## 9. Usuwanie rekordów z tabeli:

Ogólna składnia polecenia `DELETE`:
```sql
DELETE FROM tableName [WHERE condition];
```

Sposób: Usunięcie wszystkie z tabeli:
```sql
DELETE FROM tableName;
TRUNCATE tableName;
```

Usunięcie konkretnego rekordu:
```sql
DELETE FROM tableName WHERE column="value";
```
