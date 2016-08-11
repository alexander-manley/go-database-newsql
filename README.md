# go-database-newsql
Connect and manipulate a connection to CockroachDB

/* Template to link out to CockroachDB via pq driver  https://www.cockroachlabs.com/docs/build-a-test-app.html

cockroach sql -e 'CREATE DATABASE bank'

cockroach sql -e 'GRANT ALL ON DATABASE bank TO maxroach'

cockroach sql --database=bank --user=maxroach -e 'CREATE TABLE accounts (id INT PRIMARY KEY, balance INT)' 
*/

package main

import (
        "database/sql"
        "fmt"
        "log"

        _ "github.com/lib/pq"
)

func main() {
        db, err := sql.Open("postgres", "postgresql://maxroach@localhost:26257/bank?sslmode=disable")
        if err != nil {
                log.Fatalf("error connection to the database: %s", err)
        }

        // Insert two rows into the "accounts1" table.
        if _, err := db.Exec(
                "INSERT INTO accounts1 (id, balance) VALUES (5, 2000), (6, 350)"); err != nil {
                log.Fatal(err)
        }

        // Print out the balances.
        rows, err := db.Query("SELECT id, balance FROM accounts1")
        if err != nil {
                log.Fatal(err)
        }
        defer rows.Close()
        fmt.Println("Initial balances:")
        for rows.Next() {
                var id, balance int
                if err := rows.Scan(&id, &balance); err != nil {
                        log.Fatal(err)
                }
                fmt.Printf("%d %d\n", id, balance)
        }
}
