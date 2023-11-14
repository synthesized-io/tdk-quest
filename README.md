# Synthesized TDK Quest 📽️

Try to resolve the following problems using [Synthesized TDK](https://docs.synthesized.io/tdk/latest/) data generation, subsetting, and masking tools.

As the team leader in your software development company, you'll be involved in all stages of the software application life cycle. This involves the development, maintenance, and scaling of a comprehensive, modern online marketplace with an extensive network of pickup points worldwide.

## Technical requirements

Before starting the quest, we must ensure that you have some necessary tools on your PC:

- Command-line terminal such as `Powershell`, `GitBash`, or `CMD` for Windows, or your preferred terminal for Linux or macOS. Open your terminal and enter the command, replacing the `[YOUR_USERNAME]` placeholder with your name:
    
    ```bash
    echo Hello [YOUR_USERNAME]!
    ```
    
- Docker, minimum version 20.10 with Docker Compose plugin. Type in your terminal this command for showing version and existing `docker compose` command:
    
    ```bash
    docker --version
    docker compose version
    ```
    
- Git client, type in your terminal:
    
    ```bash
    git version
    ```

Consequently, you should end up with something like this:

![check-tools-version](./images/check-tools-version.gif)

Moreover, you'll need a code editor (such as VS Code) and a database client (such as [DBeaver](https://dbeaver.io/download/), [psql](https://www.postgresql.org/docs/current/app-psql.html), [DataGrip](https://www.jetbrains.com/datagrip/) etc) to connect to the test Postgres databases, navigate through the database schema, and execute simple SQL queries.

## 1. Spin up test environment 0️⃣🧪

So, your team is in the final stages of developing the new online marketplace. While the application is complete, its PostgreSQL database is currently devoid of data. This scarcity of data complicates acceptance and load testing and demonstrating the application to the customers and investors prior to production implementation. Therefore, you've been tasked with generating sufficient realistic data using [TDK generation mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/generation).

Work has already begun, and your task is to enhance the current process and scenarios.

Clone the repository with practices from GitHub:

```bash
git clone https://github.com/synthesized-io/tdk-quest.git
cd tdk-quest
git checkout init
```

Now we need two databases. The first is provided by your development team; it contains the schema of the online marketplace but no data. The second is an entirely empty database, devoid of both schema and data. This database will be used to generate fake, realistic data for the testing and management team. To retrieve both databases on your PC, you can run this simple Docker command:

```bash
docker-compose run databases
```

After the command is completed, a whale will appear in your console. This indicates that you have two databases running on ports `6000` (a database with schema only, will call it the `source database`) and `6001` (an entirely empty database for our exercises, will call it the `target database`) on your `localhost`.

Connect to both databases using your preferred database client. Use `postgres` as the `username` and `password` for both. Determine the number of tables in each database and count the number of rows in the `customer` table for each.

The existing TDK configuration file (`config_generation_from_scratch.tdk.yaml`) can already generate one fake row for each table, which includes realistic values for the columns `first_name`, `last_name`, `email`, and `username` in the `staff` table, for demonstration purposes.

To proceed, run the TDK transformation process using this configuration file:

```bash
export CONFIG_FILE=config_generation_from_scratch.tdk.yaml
docker-compose down && docker-compose run tdk
```

Once the TDK transformation is complete, connect to the output database using your database client. Verify that the schema from the source database has been copied and that there is one row in each table. You can confirm this by checking the row count in two or three randomly selected tables. Additionally, connect to the source database to ensure that it remains unchanged. Confirm that two or three randomly selected tables still have no rows.

For now, you need to enhance this data generation scenario as follows:

- Generate 100 rows for reference tables, including:
    - `country`
    - `city`
    - `category`
    - `film_category`
    - `language`
- Generate 10,000 rows for all other tables
- Ensure that realistic data is generated for the columns `first_name`, `last_name` and `email` for the `customer` table

You can run tests that cover new requirements:

```bash
docker-compose run check scan -d output_db \\
    -c /sodacl/configuration.yaml \\
    /sodacl/checks_for_generation_from_scratch.yaml
```

Ensure that there are 14 failed tests that need to be fixed (this can be found in the last line of the output log):

```bash
[13:32:39] Oops! 14 failures. 0 warnings. 0 errors. 2 pass.
```

Update the TDK configuration file and rerun the TDK with tests. If everything is correct, you will see text indicating successful tests. If not, correct the configuration file and try again.

## 2. Make test environment with production data 🏗️💽

Our application has been successfully launched and is functioning effectively. Every week, new films featuring renowned actors are released. Thousands of goods related to these films are produced and shipped to hundreds of stores, where they are sold and rented out daily by customers. And all of this information is stored as countless rows in database tables.

Given that the development team only has access to a test database with test data and not the real data from the production database, the bug fixing process becomes quite inconvenient and ineffective. For instance, if the team fixes a specific bug or optimizes performance in the test environment, there's no guarantee it will work the same way in the production environment.

In a recent team meeting, the decision was made to optimize the bug fixing and performance enhancement process. This includes creating a test database populated with real production data. This means that data from the production database would be replicated in the test database, while sensitive information, such as customer personal details and transaction records, would be anonymized. You have been designated as the person responsible for completing this task using the Synthesized TDK in Masking mode.

The detailed requirements are as follows:

- Use global KEEP mode to copy all data from the production database to the test database.
- Replace all personal customer information (like first names, last names, and emails) and personal staff information (such as first names, last names, emails, usernames, and passwords) with random, fake but realistic values. This means names should resemble real names, and emails should have a valid format. Keep all other customer and staff information unchanged.
- Replace all addresses in the `address` and `address2` columns of the related reference with fake yet realistic addresses, making sure to preserve the existing format.

## 3. **Waiting for Black Friday** 🎬🎁🖤

The sales team wants to highlight that during Black Friday, in a month's time, we'll announce up to 80% discounts. This is expected to double our customer base and quintuple our sales. As a result, we will have to increase our staff by 20%. As the head of the development team, it's your responsibility to ensure the application can handle this increased load. If necessary, make the required optimizations to accommodate the anticipated traffic

The detailed requirements are as follows:

- Use global KEEP mode to copy all data from the production database to the test database.
- Double the customer count by generating new customers.
- Increase the staff count by 20% by generating staff.
- Increase rental transactions (table `rental`) fivefold by generating new transactions.

## 4. Create a slice of data 🔪🎞️🔪

The online store efficiently handles thousands of orders per hour, 24/7. Nevertheless, the data analytics team often runs complex, time-consuming queries that could potentially slow down the store's operations and decrease profits. As a solution, it was decided to upload completed orders for the week into a separate database outside the production environment once a week. As the head of the development and operations team, your task is to upload data on all transactions from the last 7 days, once a week, while considering related data from associated tables.

The detailed requirements are as follows:

- Use global KEEP mode to copy all data from the production database to the test database.
- For the tables `rental` and `payment` copy only transaction by the last 7 days.
