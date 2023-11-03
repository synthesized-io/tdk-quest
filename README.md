# Synthesized TDK Quest

Try to resolve the following problems using Synthesized TDK data generation, subsetting, and masking tools.

## 0. Environment Preparation 🧪

Start the databases. We now have two databases running on ports `6000` and `6001` on your host.

For the initial task, use your preferred database IDE, client, or CLI to connect to both databases. Determine the number of tables in each database, and the number of rows in the `payment` table in each database.

## 1. Introduction 👋📽️

The [Pagila](https://github.com/devrimgunduz/pagila) sample database is a publicly available, educational database for PostgreSQL. It's based on the DVD rental store model and contains various tables such as films, customers, payments, and more, allowing users to practice complex queries and database management tasks.

## 2. Spin up test environment 👥0️⃣

Your team is in the final stages of developing a new application designed to manage a DVD rental network. While the application has been completed, its database currently contains minimal data, only a few rows with John and Jane Doe. This scarcity of data complicates acceptance and load testing prior to production implementation. Therefore, you've been tasked with generating sufficient realistic data from scratch for testing purposes and to demonstrate the application to the customer. The detailed requirements are as follows:

- Generate 100 rows for reference tables
- Generate 10,000 rows for other tables
- Ensure that realistic data is generated for names and addresses.

## 3. Make test environment with production data 🏗️💽

Our application has been successfully launched and is functioning effectively. Every week, new films featuring renowned actors are released. Thousands of DVDs for these films are produced and shipped to hundreds of stores, where they are rented out daily by customers for evening entertainment. All of this information is stored as countless rows in database tables.

Given that the development team only has access to a test database with test data and not the real data from the production database, the bug fixing process becomes quite inconvenient and ineffective. For instance, if the team fixes a specific bug or optimizes performance in the test environment, there's no guarantee it will work the same way in the production environment.

In a recent team meeting, the decision was made to optimize the bug fixing and performance enhancement process. This includes creating a test database populated with real production data. This means that data from the production database would be replicated in the test database, while sensitive information, such as customer personal details and transaction records, would be anonymized.

You have been designated as the person responsible for completing this task using the Synthesized TDK in Masking mode. The detailed requirements are as follows:

- Use global KEEP mode to copy all data from the production database to the test database.
- Replace all personal customer information (like first names, last names, and emails) and personal staff information (such as first names, last names, emails, usernames, and passwords) with random, fake but realistic values. This means names should resemble real names, and emails should have a valid format. Keep all other customer and staff information unchanged.
- Replace all addresses in the `address` and `address2` columns of the related reference with fake yet realistic addresses, making sure to preserve the existing format.

## 4. **Waiting for Black Friday** 🎬🎁🖤

The sales team wants to highlight that during Black Friday, in a month's time, we'll announce up to 80% discounts on DVD rentals. This is expected to double our customer base and quintuple our sales. As a result, we will have to increase our staff by 20%. As the head of the development team, it's your responsibility to ensure the application can handle this increased load. If necessary, make the required optimizations to accommodate the anticipated traffic. The detailed requirements are as follows:

- Use global KEEP mode to copy all data from the production database to the test database.
- Double the customer count by generating new customers.
- Increase the staff count by 20% by generating staff.
- Increase rental transactions (table `rental`) fivefold by generating new transactions.

## 5. Create a slice of data 🔪🎞️🔪
