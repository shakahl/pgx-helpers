# pgx-helpers

Various helpers for [`jackc/pgx`](https://github.com/jackc/pgx) PostgreSQL driver for Go.

## Scan row into struct

`ScanStruct(r *pgx.Row, dest interface{}) error`

The helper that scans a `pgx.Row` into destination struct passed by reference based on the `db` fields tags.
Implementation is heavily based on the [`jmoiron/sqlx`](https://github.com/jmoiron/sqlx) and utilises its reflection utilities.

```go
package main

import (
	"time"

	"github.com/jackc/pgx"
	pgxHelpers "github.com/vgarvardt/pgx-helpers"
)

type MyEntity struct {
    ID        string    `db:"id"`
    CreatedAt time.Time `db:"created_at"`
    SomeData  string    `db:"some_data"`
}

conn, _ := pgx.Connect(pgx.ConnConfig{...})

var result MyEntity
row := conn.QueryRow("SELECT * FROM my_entity WHERE id = $1", someID)
pgxHelpers.ScanStruct(row, &result)
```
