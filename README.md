# surexposed

[![License](https://img.shields.io/github/license/nathanfallet/surexposed)](LICENSE)
[![Issues](https://img.shields.io/github/issues/nathanfallet/surexposed)]()
[![Pull Requests](https://img.shields.io/github/issues-pr/nathanfallet/surexposed)]()
[![Code Size](https://img.shields.io/github/languages/code-size/nathanfallet/surexposed)]()
[![codecov](https://codecov.io/gh/nathanfallet/surexposed/graph/badge.svg?token=iIM9xwE4QT)](https://codecov.io/gh/nathanfallet/surexposed)

Definitions and extensions for Exposed databases.

## Installation

Add dependency to your `build.gradle(.kts)` or `pom.xml`:

```kotlin
api("me.nathanfallet.surexposed:surexposed:1.0.0")
```

```xml

<dependency>
    <groupId>me.nathanfallet.surexposed</groupId>
    <artifactId>surexposed-jvm</artifactId>
    <version>1.0.0</version>
</dependency>
```

## Usage

Define your database using the `IDatabase` interface:

```kotlin
class Database(
    protocol: String,
    host: String = "",
    name: String = "",
    user: String = "",
    password: String = "",
) : IDatabase {

    private val database: org.jetbrains.exposed.sql.Database = when (protocol) {
        "mysql" -> org.jetbrains.exposed.sql.Database.connect(
            "jdbc:mysql://$host:3306/$name", "com.mysql.cj.jdbc.Driver",
            user, password
        )

        "h2" -> org.jetbrains.exposed.sql.Database.connect(
            "jdbc:h2:mem:$name;DB_CLOSE_DELAY=-1;", "org.h2.Driver"
        )

        else -> throw Exception("Unsupported database protocol: $protocol")
    }

    override fun <T> transaction(statement: Transaction.() -> T): T = transaction(database, statement)

    override suspend fun <T> suspendedTransaction(statement: suspend Transaction.() -> T): T =
        newSuspendedTransaction(Dispatchers.IO, database) { statement() }

}
```

More coming soon...
