# Synthesized TDK Quest 📽️

In this engaging task, you'll assume the role of an online marketplace development team leader. You'll navigate through the entire application lifecycle, encountering and successfully resolving various challenges.

## Your First Working Day 👔

So, you've just taken the lead of the team, and during the first meeting, you're given all  necessary information:

- Your team is responsible for developing and maintaining a modern online marketplace
- The marketplace sells movies and other entertainment and self-development products, both online and offline
- There is a vast network of pickup points worldwide for offline sales and customer service
- From the technical perspective, the marketplace operates as a classic distributed microservice application with a central PostgreSQL database
- You have one production database and two non-production databases: development (dev) and test. These are used for developing and testing new features, as well as bug fixes
- Your team includes developers, QAs, and engineers in charge of operations and infrastructure
- The marketplace has been successfully launched and is functioning quite effectively. Every week, new films featuring renowned actors are released. Thousands of film-related goods are produced and distributed to numerous pickup points, where they are sold or rented out to customers on a daily basis. And all of this information is stored as countless rows in database tables.

## Prepare Your Workspace 🛠

As the next step, in order to set up your workspace, please make sure the required software is installed on your computer. Here are the tools you will need:

- Use your favorite command-line terminal, such as `Powershell`, `GitBash`, or `CMD` for Windows, or your preferred terminal for Linux or macOS. To verify it's working correctly, open your terminal and enter the command, replacing the `[YOUR_USERNAME]` placeholder with your name:
    
    ```bash
    echo Hello [YOUR_USERNAME]!
    ```
    
- Docker (version 20.10 or later) with the Docker Compose plugin. In your terminal, run the following command to verify your version, together with the `docker compose` command:
    
    ```bash
    docker --version
    docker compose version
    ```
    
- Git client. Run the following command to check your version:
    
    ```bash
    git version
    ```
    

Once you run those, you should end up with something like this:

![check-tools-version.gif](https://prod-files-secure.s3.us-west-2.amazonaws.com/265b146d-e896-42ff-9be9-7cc3ef184d81/c830c21e-279e-4093-8ecf-54fbc78761b0/check-tools-version.gif)

Additionally, you'll need:

- A preferred code editor for editing configuration files. If you use VS Code or any JetBrains family IDE, we strongly recommend installing the Synthesized TDK plugin for autocomplete suggestions and configuration validation. You can find related information on the "[IDE Setup](https://docs.synthesized.io/tdk/latest/user_guide/getting_started/ide_setup)" page.
- A database client (such as [DBeaver](https://dbeaver.io/download/), [psql](https://www.postgresql.org/docs/current/app-psql.html), [DataGrip](https://www.jetbrains.com/datagrip/), etc.) to connect to Postgres databases, navigate the database schema, and execute simple SQL queries.

## The First Challenge: Ship more features faster 👩‍💻

In a meeting with the company's management, you discover that the sales department aims to expedite the delivery of marketplace features by 25%. This is due to an increasing competition from other marketplaces.

The next step is to organize a brainstorming session with your team to identify the obstacles preventing the release of more features than we currently deploy. The following points were left on the marker board after an intense discussion:

- Statistics show that any given feature usually requires more than one release.
- A ready-made feature may function well in the development and test environments, but it could be completely unsuitable for the production environment. This often results in numerous re-releases:
    - For instance, if the team fixes a specific bug, optimizes performance, or adds a new feature in the test environment, there's no guarantee it will work the same way in the production environment
    - Usually, developers and QAs must investigate production issues and perform bug fixes without direct access to the production environment, essentially working in the dark
    - As a result, the team often expends considerable effort and time on re-releases, preventing the business from receiving necessary features in a timely manner
- The company's security team restricts access to the production environment, and, understandably, this decision is justified and necessary
- The main distinction between the dev/test and production environments is in the database, more specifically, in the data it contains. Currently, dev/test databases resemble data silos that contain mostly incomplete, inconsistent, outdated data, which causes issues during comprehensive testing of applications before their release

After the discussion, you decide to meet with the Ops/DBA team to discuss how to make the dev/test databases look more like production ones, and to identify any obstacles in the process. Here's what you discovered:

- Both test and dev databases have been created as copies of the production database a long time ago
- Despite the fact that the data in your databases is outdated, it still contains tons of sensitive data, such as PII and financial data
- A new copy of the production database intended for development and testing purposes is primarily made after major incidents related to the delivery and installation of new releases and patches. Currently, there are no automated processes or pipelines for these types of activities

In a recent team meeting, a decision was made to optimize the bug fixing and performance enhancement processes. Firstly, this includes creating a **test database populated with real production data**. This means that data from the production database should be periodically replicated in test/dev databases in the automation mode. Sensitive information, such as customer personal details, finance information and so on, would be anonymized. You have been assigned to complete this task using [Synthesized TDK](https://www.synthesized.io/tdk) and its [Masking mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/masking).

Therefore, you proceed as follows:

- If you're not yet familiar with Synthesized TDK in general, you should learn about it by visiting the "[What is TDK?](https://docs.synthesized.io/tdk/latest/user_guide/getting_started/what_is_tdk)" page in the official documentation
- Familiarize yourself with the [Synthesized TDK Masking mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/masking)
- To acquire auxiliary files and gain access to the necessary databases, run the following commands in your terminal:
    
    ```bash
    git clone <https://github.com/synthesized-io/tdk-quest.git>
    cd tdk-quest
    docker compose run databases
    ```
    
- Check the database connection settings (using the `postgres` value as a database name, schema name, username and password):
    - Connect to the production database using your preferred DB client. The database can be accessed from your host (`localhost` or `127.0.0.1`) via port `13600`. Make sure that this database contains a schema of the marketplace and actual data, including customers, staff, payments, and so forth
    - Connect to the empty database, which we'll refer to as the `output database`, on your server through port `13601`. Make sure that this database doesn't include any schema, database objects, or data. After completing all the steps, this database will be shared with your team's test and development personnel
- Open the `config.tdk.yaml` file in your preferred code editor. This file contains a basic Synthesized TDK configuration for masking, or in other words, anonymizing your production database data. Refer to the [documentation](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/masking) learn more about this file
- Run Synthesized TDK using this configuration file using the following command:
    
    ```bash
    docker compose run tdk
    ```
    
- Once Synthesized TDK has executed the command, connect to the output database. Ensure that this database already includes the schema and anonymized data. For instance, select any customer from the `customer` table in the production database and attempt to locate it in the output database
- You have now successfully completed the task and created a database that has the properties of a real production database, but it does not contain any sensitive data. But your teammates have requested a few more edits to the configuration file to make the resulting data more useable:
    - There's no need to conceal data like actors and films, or directories and cities, as this information isn't confidential. Apply the `KEEP` mode only for these tables
    - Use the `GENERATION` mode for the `address` and `staff` tables to make the data in the reports appear more realistic
- After making the necessary changes in the configuration file, restart the TDK transformation:
    
    ```bash
    docker compose run tdk
    ```
    
- Then, connect to the output database using your database client and verify that the result data complies with your requirements
- To thoroughly examine your output database, run tests that cover the new requirements again:
    
    ```bash
    docker compose run check_masking
    ```
    
- If everything is correct, you will see text indicating successful tests:
    
    ```bash
    [11:24:42] All is good. No failures. No warnings. No errors.
    ```
    
- If your tests are failing, update the TDK configuration file and rerun the TDK transformation and tests until you receive the `All is good` message in your terminal.

## Challenge 2: Keeping the engineering team chill during Black Friday 🎁😌🌴

The sales team plans to announce up to 80% discounts during Black Friday, which is a month away. This strategy is anticipated to significantly boost our customer base and sales during this period. Consequently, we will need to expand our staff by 20% to handle the surge in orders.

Therefore, it's crucial to ensure that all your systems can successfully handle the anticipated high load, preventing any loss of profits. As the head of the development team, it falls on you to confirm that the marketplace can withstand this increased load and to make any necessary optimizations to accommodate the expected traffic.

First, generate a realistic volume of data in development and test databases using [Synthesized TDK](https://www.synthesized.io/tdk) and its [Generation mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/generation) to begin load testing. The specific requirements include:

- If you're not yet familiar with Synthesized TDK in general, you should learn about it by visiting the "[What is TDK?](https://docs.synthesized.io/tdk/latest/user_guide/getting_started/what_is_tdk)" page in the official documentation
- Familiarize yourself with the [Synthesized TDK Generation mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/generation)
- Use the final configuration file you created in the first challenge as a starting point, and make changes to meet the following requirements:
    - Increase customer count by five times by generating new records in the `customer` table
    - Double the number of payment transactions by creating new records in the `payment` table
    - Increase the staff count by 20% by generating new records in the `staff` table
- After making the necessary changes in the configuration file, start the TDK transformation:
    
    ```bash
    docker compose run tdk
    ```
    
- Then, connect to the output database using your database client and verify that the resulted data complies with the requirements
- To thoroughly examine your output database, run tests that cover new requirements again:
    
    ```bash
    docker compose run check_generation
    ```
    
- If everything is correct, you will see text indicating successful tests:
    
    ```bash
    [11:24:42] All is good. No failures. No warnings. No errors.
    ```
    
- If your tests are failing, update the TDK configuration file and rerun the TDK transformation and tests until you receive the `All is good` message in your terminal

## Summing up the results

Congratulations! If you've reached this point, it implies that:

- You have successfully completed all assigned tasks
- Both your team and management are satisfied
- You've become proficient in the basic functionality of Synthesized TDK

We hope that Synthesized TDK will be beneficial in addressing your needs for high-quality, realistic, and secure data for testing and development. We're ready to answer all your questions and provide necessary assistance. Please feel free to email us at [support@synthesized.io](mailto:support@synthesized.io)

Additionally, we recommend joining our social media, where you can find more useful information related to test, fake, synthetic, obfuscated, anonymized, and generated data, among other related topics:

- Our [DataOps community](https://www.synthesized.io/dataops-community) on Slack
- [Twitter](https://twitter.com/synthesizedio)
- [Linkedin](https://www.linkedin.com/company/synthesized)
- [Medium](https://synthesized.medium.com/)

And finally, we ask you to fill in [a little questionnaire](https://docs.google.com/forms/d/e/1FAIpQLSdawUD6p1ld-brgG6vVJPOgGA_-yc4NPJko_rcmFHHIE19O7A/viewform?usp=sharing) that will help us become even better and solve our customers' problems even more effectively.

Thank you for your attention and support. Let's stay in touch.

Synthesized TDK Team
