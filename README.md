# Synthesized TDK Quest 📽️

Try to resolve the following problems using Synthesized TDK data generation, subsetting, and masking tools.

As the team leader in your software development company, you'll be involved in all stages of the software application life cycle. This includes development, maintenance, and scaling of a comprehensive, modern online marketplace with a broad network of pickup points.

## 1. Spin up test environment 0️⃣🧪

So, your team is in the final stages of developing the new online marketplace. While the application has been completed, its database currently contains minimal data, only a few rows with John and Jane Doe made by developers. This scarcity of data complicates acceptance and load testing and demonstrating the application to the customers and investors prior to production implementation. Therefore, you've been tasked with generating sufficient realistic data using TDK generation mode.

Work has already begun, and your task is to enhance the current process.

Clone the repo with practicies:
```bash
git clone https://github.com/synthesized-io/tdk-quest.git
cd tdk-quest
git checkout init
```

Start the databases:

```bash
docker-compose run databases
```

We now have two databases running on ports `6000` and `6001` on your host.
Use your preferred database IDE, client, or CLI to connect to both databases. Determine the number of tables in each database, and the number of rows in the `customer` table in each database.

The current TDK configuration (`config_generation_from_scratch.tdk.yaml`) already supports the following features:

- Generates 1000 rows for all tables
- Generates 10,000 rows for the `staff` table
- Generate realistic values for the columns `first_name`, `last_name`, `email` and `username` for the `staff` table

Run TDK using this configuration and inspect the output database:

```bash
export CONFIG_FILE=config_generation_from_scratch.tdk.yaml
docker-compose down && docker-compose run tdk
```

For now, you need to enhance this scenario as follows:

- Generate 100 rows for reference tables (such as `country`, `city`, `category` and so on)
- Generate 10,000 rows for all other tables (such as `rental`, `payment` and so on)
- Ensure that realistic data is generated for the columns `first_name`, `last_name` and `email` for the `customer` table

You can run tests that cover new requirements:

```bash
docker-compose run check scan -d output_db \
    -c /sodacl/configuration.yaml \
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
