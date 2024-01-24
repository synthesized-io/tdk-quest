# Synthesized TDK Quest 📽️

In this engaging task, you'll assume the role of an online marketplace development team leader. You'll navigate through the entire application lifecycle, encountering and successfully resolving various challenges.

## Your First Working Day 👔

So, you've just taken the lead of the team, and during the first meeting, you're given all the necessary information:

- Your team is responsible for developing and maintaining a modern online marketplace
- This marketplace offers users the chance to buy movies and other entertainment and self-development products, both online and offline
- There is a vast network of pickup points worldwide for offline sales and customer service
- Technically, the marketplace operates as a classic distributed microservice application with a central PostgreSQL database
- You have one production database and two non-production databases: development (dev) and test. These are used for developing and testing new features, as well as bug fixes
- Your team includes developers, QAs and engineers in charge of operations and infrastructure
- The marketplace has been successfully launched and is functioning quite effectively. Every week, new films featuring renowned actors are released. Thousands of film-related goods are produced and distributed to numerous pickup points, where they are sold or rented out to customers daily. And all of this information is stored as countless rows in database tables.


## Prepare Your Workspace 🛠

The next step involves setting up your workspace and ensuring the necessary software is installed on your computer. Conversely, you must install this software. Here are some you will need:

- Use your favorite command-line terminal, such as `Powershell`, `GitBash`, or `CMD` for Windows, or your preferred terminal for Linux or macOS. To verify it's working correctly, open your terminal and enter the command, replacing the `[YOUR_USERNAME]` placeholder with your name:
    
    ```bash
    echo Hello [YOUR_USERNAME]!
    ```
    
- Docker, minimum version 20.10 with the Docker Compose plugin. Type in your terminal this command for showing version and existing `docker compose` command:
    
    ```bash
    docker --version
    docker compose version
    ```
    
- Git client, type in your terminal:
    
    ```bash
    git version
    ```
    

Consequently, you should end up with something like this:
![check-tools-version.gif](images/check-tools-version.gif)


Additionally, you'll need:

- A preferred code editor for editing configuration files. If you use VS Code or any JetBrains family IDE, we strongly recommend installing the Synthesized TDK plugin for autocomplete suggestions and configuration validation. You can find related information on the "[IDE Setup](https://docs.synthesized.io/tdk/latest/user_guide/getting_started/ide_setup)" page.
- A database client (such as [DBeaver](https://dbeaver.io/download/), [psql](https://www.postgresql.org/docs/current/app-psql.html), [DataGrip](https://www.jetbrains.com/datagrip/), etc.) to connect to Postgres databases, navigate the database schema, and execute simple SQL queries.


## The First Challenge: Ship more features faster 👩‍💻

In a meeting with the company's management, you will discover that the sales department aims to expedite the delivery of marketplace features by 25%. This is due to the increasing competition from rival marketplaces.

The next step is to organize a brainstorming session with your team to identify the obstacles preventing the release of more features than we currently deploy. The following points were left on the marker board after this intense discussion:

- Statistics show that one feature usually requires more than one release.
- A ready-made feature may function well in a development and test environment, but it could be completely unsuitable for a production environment. This often results in numerous re-releases (new feature development).
    - For instance, if the team fixes a specific bug, optimizes performance or adds a new feature in the test environment, there's no guarantee it will work the same way in the production environment
    - Usually, developers and QAs must investigate production issues and perform bug fixes without direct access to the production environment, essentially working in the dark (production issue investigation)
    - As a result, the team often expends considerable effort and time on re-releases, preventing the business from receiving necessary features in a timely manner (shorten time to market)
- The company's security service restricts access to the production environment, and understandably, this decision is justified and necessary (reduce data privacy risks).
- The main distinction between the dev/test and production environments lies in the database, particularly the data it contains. Currently, dev/test databases resemble data silos, containing mostly incomplete, inconsistent, and outdated data, which hinders comprehensive testing of applications before their release (silos that decrease development velocity).

After the discussion, you decided to meet with the Ops/DBA team to discuss how to make the dev/test databases more like production ones and to identify any obstacles in this process. Here's what you discovered:

- Both test and dev databases has been created as copy from the production database long time ago (silos that decrease development velocity)
- Despite data in your databases are very non actually they still contain tons of sensitive data anyway, such as PII and financial data (reduce data privacy risks)
- The next copy of the production database for development and testing is primarily made after major incidents related to the delivery and install of new releases and patches. Currently, there are no automated processes or pipelines for these types of activities (manual operations for preparing environments)

In a recent team meeting, the decision was made to optimize the bug fixing and performance enhancement process. Firstly, this includes creating a **test database populated with real production data**. This means that data from the production database should be periodically replicated in automation mode in test/dev databases, while sensitive information, such as customer personal details, finance information and so on, would be anonymized. You have been assigned to complete this task using [Synthesized TDK](https://www.synthesized.io/tdk) and its [Masking mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/masking).

Therefore, you should proceed as follows:

- If you're not yet familiar with Synthesized TDK in general, you should learn about it by visiting the "[What is TDK?](https://docs.synthesized.io/tdk/latest/user_guide/getting_started/what_is_tdk)" page in the official documentation
- Familiarize yourself with the [TDK Masking mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/masking)
- To acquire auxiliary files and gain access to the necessary databases, type the following commands into your terminal:
    
    ```bash
    git clone https://github.com/synthesized-io/tdk-quest.git
    cd tdk-quest
    docker compose run databases
    ```
    
- Check connections (using the `postgres` value as a database name, schema name, username and password):
    - Connect to the production database using your preferred DB client. The database can be accessed from your host (`localhost` or `127.0.0.1`) via port `6000`. Make sure that this database contains a schema of the marketplace and actual data, including customers, staff, payments, and so forth
    - Connect to the empty database, which we'll refer to as the `output database`, on your host through port `6001`. Make sure that this database doesn't include any schema, database objects, or data. After completing all the steps, this database will be shared with your team's test and development personnel
- Open the `config.tdk.yaml` file in your preferred code editor. This file contains a basic Synthesized TDK configuration for masking, or in other words, anonymizing your production database data. Refer to the [documentation](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/masking) for further exploration of this file
- Run Synthesized TDK using this configuration file by running the following command:
    
    ```bash
    docker compose run tdk
    ```
    
- Once Synthesized TDK has completed its task, connect to the output database. Ensure that this database already includes the schema and anonymized data. For instance, select any customer from the `customer` table in the production database and attempt to locate it in the output database
- In general, we have successfully completed the task and created a database that has the properties of a real production database, but it does not contain any sensitive data. But your teammates have requested a few more edits to the configuration file to enhance the convenience of working with the resulting data:
    - There's no need to conceal data like actors and films, or directories and cities, as this information isn't confidential. Apply the `KEEP` mode only for these tables
    - Use the `GENERATION` mode for the `address` and `staff` tables to make the data in the reports appear more realistic
- After making the necessary changes in the configuration file, restart the TDK transformation:
    
    ```bash
    docker compose run tdk
    ```
    
- Then, connect to the output database using your database client and verify that the resulted data complies with the requirements
- To thoroughly examine your output database, run tests that cover new requirements again:
    
    ```bash
    docker compose run check_masking
    ```
    
- If everything is correct, you will see text indicating successful tests:
    
    ```bash
    [11:24:42] All is good. No failures. No warnings. No errors.
    ```
    
- If you still have failing tests, update the TDK configuration file and rerun the TDK transformation and tests until you receive the `All is good` message in your terminal.



## Challenge 2: Keeping the engineering team chill during Black Friday 🎁😌🌴

The sales team plans to announce up to 80% discounts during Black Friday, which is a month away. This strategy is expected to double our customer base and increase our sales fivefold during this period. Consequently, we will need to expand our staff by 20% to handle the surge in orders.

Therefore, it's crucial to ensure that all your systems can successfully handle the anticipated high load, preventing any loss of profits. As the head of the development team, it falls on you to confirm that the marketplace can withstand this increased load and to make any necessary optimizations to accommodate the expected traffic.

TODO (load testing)

Firstly, you need to generate a realistic amount of data on development and test databases using [Synthesized TDK](https://www.synthesized.io/tdk) and its [Generation mode](https://docs.synthesized.io/tdk/latest/user_guide/tutorial/generation). The specific requirements are as follows:

- Use global MASKING mode to copy all data from the production database to the test database
- Double the customer count by generating new records for the `customer` table
- Increase the staff count by 20% by generating staff new records for the `staff` table
- Increase payment transactions fivefold by generating new records for the `payment` table
