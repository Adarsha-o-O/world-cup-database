# World Cup Database

A PostgreSQL database project that stores FIFA World Cup knockout-stage match results and uses SQL queries to analyze teams, goals, and tournament outcomes.

This project was completed as part of the **[freeCodeCamp Relational Database Certification](https://www.freecodecamp.org/learn/relational-database/)** challenge, **Build a World Cup Database**.

## Project Overview

The project uses PostgreSQL to store match results and Bash scripts to import match data and run SQL queries.

The dataset contains **32 knockout-stage matches** from the 2014 and 2018 FIFA World Cups, with 16 matches from each tournament. It is a limited dataset for the challenge, not a complete record of every match played in either tournament.

## Features

- Store World Cup teams and match results in a relational database.
- Import match data from a CSV file using Bash and PostgreSQL.
- Avoid inserting duplicate teams during data import.
- Connect match records to teams using foreign keys.
- Analyze match results with SQL aggregate functions and filtering.
- Query tournament outcomes, team names, and goal statistics.
- Support the challenge's test database through the import script's `test` argument.

## Technologies Used

- **PostgreSQL** — relational database and SQL queries
- **Bash** — scripts for importing data and executing queries
- **CSV** — source file for match results
- **Git and GitHub** — version control and project hosting

## Repository Structure

```text
world-cup-database/
├── games.csv
├── insert_data.sh
├── queries.sh
├── worldcup.sql
└── README.md
```

| File | Description |
|---|---|
| `games.csv` | Match data containing the year, round, teams, and goals scored. |
| `insert_data.sh` | Reads the CSV file, creates missing team records, and inserts match records into PostgreSQL. |
| `queries.sh` | Runs SQL queries to analyze match results and tournament statistics. |
| `worldcup.sql` | PostgreSQL database dump containing the database schema and stored data. |
| `README.md` | Project documentation and setup instructions. |

## Database Design

The database contains two main tables: `teams` and `games`.

### 1. `teams`

Stores the participating teams.

| Column | Purpose |
|---|---|
| `team_id` | Unique identifier for each team; primary key. |
| `name` | Team name; unique value. |

### 2. `games`

Stores match results.

| Column | Purpose |
|---|---|
| `game_id` | Unique identifier for each game; primary key. |
| `year` | Year of the tournament. |
| `round` | Stage of the tournament. |
| `winner_id` | References the winning team in the `teams` table. |
| `opponent_id` | References the opposing team in the `teams` table. |
| `winner_goals` | Goals scored by the winning team. |
| `opponent_goals` | Goals scored by the opposing team. |

The `winner_id` and `opponent_id` columns use foreign keys to reference teams. This avoids repeatedly storing full team names in every match record.

## How It Works

### 1. Import match data

The `insert_data.sh` script reads `games.csv` line by line and skips its header. For each match, it:

1. Checks whether the winning team already exists in the `teams` table.
2. Inserts the team if it does not exist.
3. Repeats the process for the opposing team.
4. Retrieves the team IDs.
5. Inserts the match result into the `games` table.

This approach helps prevent duplicate team records when multiple matches involve the same teams.

### 2. Analyze match results

The `queries.sh` script executes SQL queries against the `worldcup` database to retrieve statistics and answer questions about the stored match results.

## SQL Queries and Analysis

The project includes queries to calculate or retrieve:

- Total goals scored by winning teams.
- Total goals scored by both teams across all matches.
- Average goals scored by winning teams.
- Average winning-team goals rounded to two decimal places.
- Average total goals per match.
- Highest number of goals scored by either team in a match.
- Number of matches in which the winning team scored more than two goals.
- The winner of the 2018 final.
- Teams that participated in the 2014 eighth-final round.
- Unique names of winning teams.
- Tournament champions and their corresponding years, based on the stored final-match results.
- Team names beginning with `Co`.

These queries demonstrate the use of SQL aggregate functions, filtering, grouping, joins, and relational database constraints.

## Getting Started

### Prerequisites

Install or have access to:

- PostgreSQL
- `psql`, the PostgreSQL command-line client
- Bash
- Git

The commands below assume a local PostgreSQL installation and the usernames and database names used by the challenge scripts.

### 1. Clone the repository

```bash
git clone https://github.com/Adarsha-o-O/world-cup-database.git
cd world-cup-database
```

### 2. Review the database dump

The `worldcup.sql` file contains SQL statements to drop and recreate the `worldcup` database, along with its schema and data.

**Warning:** Restoring this dump can delete an existing database named `worldcup`. Back up any important data before running it.

If you intend to restore the dump and understand its effects, run:

```bash
psql -U postgres -d postgres -f worldcup.sql
```

The restore requires suitable PostgreSQL permissions and may prompt for a password depending on your configuration.

### 3. Configure database access

The scripts use the following database configurations:

| Script mode | Username | Database |
|---|---|---|
| Normal import | `freecodecamp` | `worldcup` |
| Test import (`test`) | `postgres` | `worldcuptest` |
| Queries | `freecodecamp` | `worldcup` |

Ensure PostgreSQL is running and that the required databases, users, and permissions are configured. Adjust the connection settings in the scripts if your local environment uses different credentials.

### 4. Make the scripts executable

```bash
chmod +x insert_data.sh queries.sh
```

### 5. Import the match data

To import the CSV data into the main database:

```bash
./insert_data.sh
```

To run the import script in test mode:

```bash
./insert_data.sh test
```

The test argument changes the import script's connection settings to use `worldcuptest` with the `postgres` username. The test database must already exist and be configured appropriately.

### 6. Run the SQL queries

```bash
./queries.sh
```

This executes the project's analysis queries against the `worldcup` database.

## Concepts Practiced

This project provided practice with:

- Designing relational database schemas.
- Creating primary keys and foreign key relationships.
- Enforcing unique values.
- Importing structured data from CSV files.
- Writing Bash scripts that execute PostgreSQL commands.
- Using `SELECT`, `INSERT`, `WHERE`, joins, and aggregate functions.
- Analyzing data with `COUNT`, `SUM`, `AVG`, `MAX`, and rounding.
- Working with command-line database tools.
- Managing database setup and test configurations.

## Current Scope and Possible Improvements

The current dataset covers 32 knockout-stage matches from the 2014 and 2018 tournaments.

Possible future improvements include:

- Expand the dataset to cover group-stage matches and additional tournaments.
- Add validation and clearer error handling during CSV import.
- Improve configuration so database connection details are easier to change.
- Add automated tests for import behavior and query results.
- Document sample query outputs.

These are potential improvements, not claims about existing functionality.

## Author

**Adarsha**

- GitHub: [@Adarsha-o-O](https://github.com/Adarsha-o-O)

---

This project was completed as part of the freeCodeCamp Relational Database Certification curriculum.
