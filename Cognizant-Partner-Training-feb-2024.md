Cognizant Partner Training
​
### Helpful resources
​
**[Synthesized Docs](https://docs.synthesized.io)
​
**[Synthesized.io](https://synthesized.io)
​
**[Demo’s on Github](https://github.com/synthesized-io/tdk-demo)
​
**Test your knowledge [**>>**](https://docs.google.com/forms/d/e/1FAIpQLSezW13M-ySxp7mDW2EC2a2pw169Jbjnu9TrVOeU7VkDE5DdYg/viewform)
​
**[TDK Quest](https://docs.synthesized.io/tdk-quest/)
​
---
​
## Agenda
​
1. **Introduction to Synthesized TDK with Nicolai**
2. **User Interface and basic workflow with Margarita** 
    1. Create first workflow
        - Create Datasources
        - Create Workflows
        - Create default masking configuration
        - Compare source and masked data
3. **Key Functionalities with Margarita**
    1. TDK three main modes
        1. MASKING
        2. GENERATION
        3. SUBSETTING
4. **Quick Break & Intro to TDK Quest** 
5. **YAML structure basics** 
    1. YAML basics
    2. TDK YAML configuration structure
6. **TDK  transformations with Denis** 
    - Person Generator
    - Address Generator
    - Categorical generator
    - Format Preserving Hashing
    - Passthrough
    - Data Filtering
    - Documentation Tutorials
    - Demos Repository
7. **TDK interfaces with Denis** 
    1. Web UI
    2. REST: overview using Postman
    3. CLI: installation, configuration, running
8. **Deployment options with Denis** 
    - Requirements and configuration of necessary software tools
        - Java 17
        - Docker
    - See how to deploy Synthesized TDK using:
        - jar
        - Docker
        - Kubernetes
9. **TDK Integration and Deployment with Denis** 
    1. Integrating TDK with existing CI/CD tools
    2. Deployment considerations and best practices 
    3. Database User Permissions
10. **TDK Troubleshooting and Support with Denis** 
    1. Troubleshooting for TDK 
    2. Available resources and support channels
11. **Q&A and Wrap-up** 
    1. Open discussion and addressing any remaining questions
​
## Demo configurations:
​
### KEEP mode**. The following configuration copies database objects and data from the demo schema to the output database without masking:
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
```
​
### GENERATION mode**. The following configuration learns data from the input schema `public` and generates a database twice the size (with `target_ratio: 2`), with similar distributions and automatically preserved referential integrity:
​
```yaml
default_config:
  mode: GENERATION
  target_ratio: 2
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### MASKING mode**. The following configuration creates a masked copy of the input schema `public`, automatically preserving referential integrity:
​
```yaml
default_config:
  mode: MASKING
  target_ratio: 1
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### GENERATION mode with subsetting**. 
​
```yaml
default_config:
  mode: GENERATION
  target_ratio: 0.5
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### MASKING mode with subsetting**.
​
```yaml
default_config:
  mode: MASKING
  target_ratio: 0.5
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### SUBSETTING FOR SPECIFIC TABLES:**
​
```sql
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: 'public.rental'
    target_ratio: 0.1
​
  - table_name_with_schema: 'public.payment'
    target_ratio: 0.1
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### GENERATION with MASKING and KEEP**. 
​
```sql
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: 'public.staff'
    mode: MASKING
​
  - table_name_with_schema: 'public.customer'
    mode: GENERATION
    target_ratio: 2
​
  - table_name_with_schema: 'public.payment'
    mode: GENERATION
    target_ratio: 2
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Person Generator**.
​
```sql
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: 'public.staff'
    mode: MASKING
    transformations:
      - columns: ["last_name", "first_name", "email"]
        params:
          type: person_generator
          column_templates: ["${last_name}", "${first_name}", "${email}"]
          locale: fr
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Categorical Generator**. 
​
```sql
default_config:
  mode: GENERATION
  target_ratio: 2
tables:
  - table_name_with_schema: "public.film"
    transformations:
      - columns: ["rating"]
        params:
          type: "categorical_generator"
          categories:
            type: STRING
            value_source: PROVIDED
            values:
              "PG-13": 0.223
              "NC-17": 0.21
              "R": 0.195
              "PG": 0.194
              "G": 0.178
              "NEW": 0.5
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Format Preserving Hashing.** 
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: 'public.address'
    mode: MASKING
    transformations:
      - columns: ['phone']
        params:
          type: format_preserving_hashing
          filter:
            type: 'last'
            n: 5
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Passthrough**. 
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: 'public.address'
    mode: MASKING
    transformations:
      - columns: ['postal_code', 'district']
        params:
          type: passthrough
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Ignore tables:**
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 0
​
tables:
  - table_name_with_schema: 'public.film'
    target_ratio: 1
  - table_name_with_schema: 'public.language'
    target_ratio: 1
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: 'public.payment'
    target_ratio: 0
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Data Filtering**. 
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
​
tables:
  - table_name_with_schema: "public.category"
    filter: name like '%Drama%'
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Data Filtering Complex Example.** 
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
tables:
  - table_name_with_schema: "public.category"
    filter: name like '%Drama%'
  
  - table_name_with_schema: "public.language"
    filter: name like '%French%'
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
​
### Filter by one user:**
​
```yaml
default_config:
  mode: KEEP
  target_ratio: 1
tables:
  - table_name_with_schema: "public.customer"
    mode: MASKING
    filter: customer_id = 1
​
schemas: ["public"]
schema_creation_mode: DROP_AND_CREATE
safety_mode: RELAXED
```
