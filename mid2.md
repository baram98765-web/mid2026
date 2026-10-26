# 2번 답안

```
#include <stdio.h>
#include <stdlib.h>
#include "sqlite3.h"

int callback(void *NotUsed, int argc, char **argv, char **azColName) {
    for (int i = 0; i < argc; i++) {
        printf("%s = %s\n", azColName[i], argv[i] ? argv[i] : "NULL");
    }
    printf("--------------------\n");
    return 0;
}

int main(void) {
    sqlite3 *db;
    char *errMsg = 0;
    int rc;

    // 1. Open Database
    rc = sqlite3_open("school.db", &db);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "Can't open database: %s\n", sqlite3_errmsg(db));
        sqlite3_close(db);
        return 1;
    }

    // 2. Create Table
    const char *sqlCreateTable = "DROP TABLE IF EXISTS students;"
                                 "CREATE TABLE students(id INT, name TEXT, score INT);";
    rc = sqlite3_exec(db, sqlCreateTable, 0, 0, &errMsg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", errMsg);
        sqlite3_free(errMsg);
        sqlite3_close(db);
        return 1;
    }

    // 3. Insert Data (3 or more students)
    const char *sqlInsert = "INSERT INTO students VALUES(1, 'Alice', 85);"
                            "INSERT INTO students VALUES(2, 'Bob', 92);"
                            "INSERT INTO students VALUES(3, 'Charlie', 78);";
    rc = sqlite3_exec(db, sqlInsert, 0, 0, &errMsg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", errMsg);
        sqlite3_free(errMsg);
        sqlite3_close(db);
        return 1;
    }

    // 4. Search Implementation (Search student named 'Bob')
    printf("=== Search Result (Name: Bob) ===\n");
    const char *sqlSearch = "SELECT * FROM students WHERE name = 'Bob';";
    rc = sqlite3_exec(db, sqlSearch, callback, 0, &errMsg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", errMsg);
        sqlite3_free(errMsg);
        sqlite3_close(db);
        return 1;
    }

    // 5. Close Database
    sqlite3_close(db);
    return 0;
}
```

<img width="1115" height="628" alt="image" src="https://github.com/user-attachments/assets/1df6e7d3-dfd0-436a-a9a4-c94797948a57" />

