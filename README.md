# Semantic View Package
---
## Semantic View and Semantic Query - Brief Summary

A Semantic View is a logical layer built on top of multiple database tables to make data easier to understand and query. Instead of writing complex SQL with many joins and aggregations, users can query the semantic view using simple business terms like dimensions and metrics.

It defines how tables are related, which columns represent descriptive attributes (dimensions), and which represent measurable values (metrics or facts). The semantic view hides the underlying complexity of joins, keys, and calculations, and presents a clean analytical model.

In simple terms, a semantic view acts like a business-friendly data model that allows analysts or tools to query data without needing to know the full database structure. It is especially useful for reporting, dashboards, and analytics because it ensures consistent calculations and relationships across queries.

Within a semantic view, you define logical tables that typically correspond to business entities, such as customers, orders, or suppliers. You can define relationships between logical tables through joins on shared keys, enabling you to analyze data across entities (as you would when joining database tables).

Using logical tables, you can define:

**Facts**: Facts are row-level attributes in your data model that represent specific business events or transactions. While facts can be defined using aggregates from more detailed levels of data (such as SUM(t.x) where t represents data at a more detailed level), they are always presented as attributes at the individual row level of the logical table. Facts capture “how much” or “how many” at the most granular level, such as individual sales amounts, quantities purchased, or costs. It’s important to note that facts typically function as “helper” concepts within the semantic view to help construct dimensions and metrics.

**Metrics**: Metrics are quantifiable measures of business performance calculated by aggregating facts or other columns from the same table (using functions like SUM, AVG, and COUNT) across multiple rows. They transform raw data into meaningful business indicators, often combining multiple calculations in complex formulas. Examples include Total Revenue or Profit Margin Percentage. Metrics represent the KPIs in reports and dashboards that drive business decision-making.

**Dimensions**: Dimensions represent categorical attributes. They provide the contextual framework that gives meaning to metrics by grouping data into meaningful categories. They answer “who,” “what,” “where,” and “when” questions, such as purchase date, customer details, product category, or location. Typically text-based or hierarchical, dimensions enable users to filter, group, and analyze data from multiple perspectives.

In a semantic view, these three elements have distinct roles, but metrics and dimensions are the primary elements that you interact with when analyzing data through the semantic view. Facts provide the underlying row-level numerical data, metrics transform data into actionable insights through aggregation and calculation, and dimensions determine viewing perspectives.

### Package Includes
* [Semantic View](#semantic-view)
* [Semantic Query](#semantic-query)

## Semantic View 

### Semantic View Node Configuration
* [Node properties](#semantic-view-node-properties)
* [Semantic View Options](#semantic-view-options)

### Node properties

| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Semantic View will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

### Semantic View Options 

| **Property** | **Description** |
|---------|-------------|
| Create Schema Table | Toggle: True/False<br/>`True`: Creates a schema table for the Semantic View. Required when using a Semantic Query node so downstream nodes can fetch and use columns.<br/>`False`: Semantic View is created |
| Materialization | Defines how the semantic model is created.<br/>Options:<br/>- Semantic View |
| Consider Table Only (No Primary Key) | Toggle: True/False<br/>`True`: Builds only table names<br/>`False`: Builds table names along with Primary keys |
| TABLES (Primary Key) | Tabular configuration for defining source tables and their Primary Key columns.<br/>- `Source Column`: Select the primary key column from the source table.<br/>- `Alias`: Optional alias name for the column.<br/>- `Synonym`: Optional alternative names for the column.<br/>- `Comment`: Optional description for the column. |
| Enable Relationships | Toggle: True/False<br/>`True`: Enables the RELATIONSHIPS (Foreign Key) configuration section.<br/>`False`: Relationship configuration remains hidden. |
| RELATIONSHIPS (Foreign Key) | Tabular configuration for defining foreign key relationships between tables.<br/>- `Source Column`: Column acting as the foreign key.<br/>- `Reference Table`: Table being referenced.<br/>- `Alias`: Optional alias for the reference column. |
| Enable Facts | Toggle: True/False<br/>`True`: Enables the FACTS configuration section.<br/>`False`: Facts configuration remains hidden. |
| FACTS | Tabular configuration for defining fact columns or measures.<br/>- `Derived Stage`: Processing stage for derived columns (1, 2, or 3). Visible only when schema table creation is enabled.<br/>- `Source Column`: Column selected from the source table.<br/>- `Transform`: Expression applied on the column.<br/>- `Alias`: Name used in the semantic model.<br/>- `Synonym`: Optional alternative names.<br/>- Comment: Optional description. |
| Enable Dimensions | Toggle: True/False<br/>`True`: Enables the DIMENSIONS configuration section.<br/>`False`: Dimensions configuration remains hidden. |
| DIMENSIONS | Tabular configuration for defining dimension attributes.<br/>- `Derived Stage`: Processing stage for derived columns (1, 2, or 3). Visible only when schema table creation is enabled.<br/>- `Source Column`: Column selected from the source table.<br/>- `Transform`: Expression applied to the column.<br/>- `Alias`: Name used in the semantic model.<br/>- `Synonym`: Optional alternative names.<br/>- `Comment`: Optional description. |
| Enable Metrics | Toggle: True/False<br/>`True`: Enables the METRICS configuration section.<br/>`False`: Metrics configuration remains hidden. |
| METRICS | Tabular configuration for defining aggregated metrics.<br/>- `Derived Stage`: Processing stage for derived columns (1, 2, or 3). Visible only when schema table creation is enabled.<br/>- `Metric Applied`: Aggregation function applied to the column (COUNT, SUM, AVG, MIN, MAX).<br/>- `Source Column`: Column used for aggregation.<br/>- `Derived Column`: Optional custom expression for metric calculation.<br/>- `Alias`: Name used for the metric in the semantic model.<br/>- `Synonym`: Optional alternative names.<br/>- `Comment`: Optional description. |

### Semantic View Browser Creation Steps

1. **Enable "Create Schema Table"**
   - Turn **Create Schema Table** toggle **ON**.
   - Set the Derived Stage dropdown to the appropriate level based on the columns used in the expression.
   - This creates an intermediate schema table that helps fetch and manage columns during configuration.

2. **Configure Required Components**
   Enter the necessary configurations depending on your use case:
   - **Primary Keys** under *TABLES (Primary Key)*
   - **Relationships** under *RELATIONSHIPS (Foreign Key)* (if required)
   - **Facts**
   - **Dimensions**
   - **Metrics**

3. **Create Table in Browser**
   - Use **Create Table in Browser** to generate the schema table.
   - This allows the columns to be available for further configuration and downstream usage.

4. **Resync Columns**
   - After successful table creation, click **Resync Columns**.
   - This fetches the generated columns and updates the node metadata.

5. **Disable "Create Schema Table"**
   - Turn **Create Schema Table** toggle **OFF**.
   - This switches the node to generate a **Semantic View** instead of a schema table.

6. **Create Semantic View**
   - Deploy or create the **Semantic View** using the same configuration already defined.

7. **Important Note for Deployment**
   - Before deploying to the **target environment**, ensure **Create Schema Table** is **OFF**.
   - If left **ON**, the process may recreate the schema table instead of the semantic view.

### Limitations

- This feature currently creates only **basic-level semantic nodes**.

- Complex semantic models that require **advanced transformations or multi-step logic** may not be supported at this time. Support for these scenarios is planned in future updates.

- **Validation must be completed before deployment** to ensure the semantic view definition is valid.

- For the full set of **validation rules and constraints**, please refer to [the official Snowflake Semantic View documentation](https://docs.snowflake.com/en/user-guide/views-semantic/validation-rules).

## Semantic View  Deployment

### Semantic View  Initial Deployment

When deployed for the first time into an environment the Semantic View  node will execute the following stage:

| **Stage** | **Description** |
|-----------|----------------|
| Create  Semantic View | This stage will execute a `CREATE OR REPLACE` statement and create a Semantic View in the target environment. |

#### Semantic View  Redeployment

After initial deployment, subsequent deployments recreates the Semantic View.

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment,then no stages are executed

### Node Type Switching

Node Type switching is supported starting from Coalesce version **7.28+**.

From this version onward, a node’s materialization type can be switched from one supported type to another, subject to certain limitations.

For more info click here - [Node Type Switching Logic and Limitations](#node-type-switching-logic)

### Semantic View  Undeployment

A table will be dropped if all of these are true:

* The Semantic View Node is deleted from a space.
* The space is committed to Git.
* The space committed to Git is deployed to a higher level environment.

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Semantic View** | Removes object from target environment |

------

### Semantic Query Node Configuration
* [Node properties](#semantic-query-node-properties)
* [Semantic Query Options](#semantic-query-options)

### Node properties
| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Semantic Query will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

### Semantic Query Options 

| **Property** | **Description** |
|---------|-------------|
| Materialization | Defines how the semantic query output is created.<br/>Options:<br/>- `view` |
| Order By | Toggle: True/False<br/>`True`: Enables sorting configuration for the semantic query.<br/>`False`: No ordering is applied. |
| Order By Columns | Tabular configuration for defining sorting columns. Visible only when Order By is enabled.<br/>- `Sort Column`: Column used for sorting.<br/>- `Sort Order`: Sorting direction.<br/>Options:<br/>- DESC (default)<br/>- ASC |
| FACTS | Column selector used to choose fact columns from the semantic view for the query output. |
| DIMENSIONS | Column selector used to choose dimension columns from the semantic view for the query output. |
| METRICS | Column selector used to choose metric columns from the semantic view for the query output. |

## Semantic Query  Deployment

### Semantic Query  Initial Deployment

When deployed for the first time into an environment the Semantic Query  node will execute the following stage:

| **Stage** | **Description** |
|-----------|----------------|
| Create Semantic Query View | This stage will execute a `CREATE OR REPLACE` statement and create a view in the target environment. |

#### Semantic Query  Redeployment

After initial deployment, subsequent deployments recreates the Semantic Query View with default deployment startegy

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment, then no stages are executed

### Node Type Switching

Node Type switching is supported starting from Coalesce version **7.28+**.

From this version onward, a node’s materialization type can be switched from one supported type to another, subject to certain limitations.

For more info click here - [Node Type Switching Logic and Limitations](#node-type-switching-logic)

### Semantic Query  Undeployment

A table will be dropped if all of these are true:

* The Semantic Query Node is deleted from a space.
* The space is committed to Git.
* The space committed to Git is deployed to a higher level environment.

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Semantic Query** | Removes object from target environment |

---

#### Node Type Switching Logic
| Current MaterializationType | Desired MaterializationType | Stage |
|------------|--------|-------|
| Semantic View | Semantic View | Follows existing redeployment stages |
| Any Other | Semantic View | 1. Warning (if applicable)<br/>2. Drop <br/> 3. Create |
| Any  | View(Semantic Query) | May recreate with the default deployment strategy, but it might not work as expected |

Please review the documented limitations before performing a node type switch to ensure compatibility and avoid unintended deployment issues.

#### ⚠ Limitations of Node Type Switching (Current)

| # | Current Materialization | Desired Materialization | Limitation |
|---|--------------------------|--------------------------|------------|
| 1 | Older Version Iceberg Table | Table | Results in `ALTER` failure. Iceberg tables require `ALTER ICEBERG TABLE`. Works only if latest package (with switching support) is already used. |
| 2 | Older Version<br/>Create or Alter-View<br/>Data Quality-DMF | Any(except View) | Switch fails unless current node uses latest package supporting node type switching. |
| 3 | First Node in Pipeline | Any | Not supported. First node is foundational and switching may disrupt the pipeline. |
| 4 | External Packages | Any | Not supported as they typically act as first nodes in the pipeline. |
| 5 | Functional Packages | Any | Not supported due to column re-sync behavior which may cause schema inconsistencies. |
| 6 | Dynamic Dimension / LRV | Any | System columns must be manually dropped before redeployment. |
| 7 | Any | Any Other | After performing node switching, the `Create/Run` in Workspace browser may not work as expected due to changes in the node’s materialization type. |
| 8 | Table(Data Profiling) | Table | This may result in ALTER failure unless latest package is used(with system column removal support)**(Pending Release)** |
| 9 | Any | Any Stream-based Node (Stream, Stream & I/M, Delta Merge, or Directory Stream) | When switching to a Stream-based node, do not select **'Create At Existing Stream'** from the Redeployment Behavior; this causes deployment errors. Use **'Create or Replace'** or **'Create If Not Exists'**. |
| 10 | Stream | Stream for Directory Table (and vice versa) | Metadata columns are not automatically synchronized. Specific directory columns (e.g., `relative_path`, `size`, `md5`) are not added when switching to Directory Table, nor are they removed when switching back to a standard Stream. |
| 11 | Stream | Any Other (and vice versa) | Snowflake CDC metadata columns (`METADATA$ACTION`, `METADATA$ISUPDATE`, `METADATA$ROW_ID`) are not automatically managed. They are neither removed nor added when there's a node type switch |
| 12 | Any | View(Semantic Query) | May recreate it with the default deployment strategy, but it might not work as expected since semantic queries are only valid when the source is a Semantic Query. |

--------------

## Code

### Semantic View Code
* [Node definition]()
* [Create Template]()
* [Run Template]()

* ### Semantic Query Code
* [Node definition]()
* [Create Template]()
* [Run Template]() 
