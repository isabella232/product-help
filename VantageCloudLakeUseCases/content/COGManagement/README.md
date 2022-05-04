## Compute Group Management with SQL

### Introduction

The following is a summary of how to manage COGs in Vantage CloudLake with SQL.


### Create a resource group for new research project

A COG group contains one or more COGs and is used to form an association between the users and the resources for the user's queries.

The Query+Strategy policy is provided for future use.

```sql
CREATE COG GROUP Research_Group USING QUERY_STRATEGY ('STANDARD');
```

### Creating roles for the project

Two roles are provided. 
* An admin role to administrate the compute-group and the users in the group
* A user role which permits access to the COG resources


```sql
-- Create an admin role for the project
CREATE ROLE Research_Admin_Role;
GRANT CREATE COG GROUP TO Research_Admin_Role;
GRANT DROP COG GROUP TO Research_Admin_Role;

-- Create the user role for the project
CREATE ROLE Research_Role;
GRANT COG GROUP Research_Group TO Research_Role;
```

### Associating users with COG resources

Users can access COG resources by having the user associated with a COG group.
The user can be created or modified to have a default association.
The user also can just be granted access and set it directly.

```sql
-- Create a new user
CREATE USER "${USER}" AS 
PASSWORD = "${PASSWORD}"
PERM=1e8
DEFAULT DATABASE = "${USER}"
COG GROUP = Research_Group;

-- Provide default COG resources to an existing user
MODIFY USER "${USER}" AS COG GROUP = Research_Group;

-- Also provide access to the COG resources
GRANT Research_Role TO "${USER}";

-- This user also can administrate the resources
GRANT Research_Admin_Role TO "${USER}";

-- User can set access to any specific group that they have access
SET SESSION COG GROUP Research_Group;
```

### Assign resources to a group

A COG profile describes the resource policy.
A COG consists of one or more instances defined using min and max.
Min represents always available count of instances or rank.
Max represents the elastic portion of instances that can be used.
Each instance represents a number of VM nodes such as EC2 instances. 
The number of VM nodes is controlled by the instance size descriptor such as Small, Medium, Large.
Each size has twice the number of VM nodes over the previous size: 2, 4, 8, 16, etc.
There are other options that can be specified for the policy
* initially_suspended hibernates the COG after configuration until the user manually RESUMEs the COG
* Start and End times can be specified for when the COG is operational using cron tab format
* cooldown_period specifies how long the COG continues to run after the End time so that queries can complete

```sql
-- View existing maps for available COG sizes
SELECT * FROM DBC.COGMAPSV ORDER BY NodeCount;

-- Provide resources for group
-- $MIN = 1
-- $MAX = MIN to X
-- $COOLDOWN = 1 to Y minutes
-- Create COGs where X=2 and Y=30 minutes

CREATE COG PROFILE Research_Resources
IN Research_Group, INSTANCE = TD_COG_SMALL
USING
MIN_COG_COUNT  ( 1 ) MAX_COG_COUNT  ( 2 ) SCALING_POLICY  ('STANDARD') INSTANCE_TYPE  ('STANDARD') 
INITIALLY_SUSPENDED  ('FALSE') START_TIME  ('') END_TIME  ('') COOLDOWN_PERIOD  ( 30 );
```

### Query COG dictionary & Get status about the COG
 
```sql 
-- View existing maps for available COG sizes
SELECT * FROM DBC.COGMAPSV ORDER BY NodeCount;

-- Display COG Group COG Policies
SELECT * FROM DBC.COGGROUPSV;

-- Similar to the DBC.COGGROUPV view but it displays COG group details for COG groups to which the user has access
SELECT * FROM DBC.COGGROUPSVX;

-- Provide COG state information
SELECT * FROM DBC.COGPROFILESV;

-- Similar to the DBC.COGV view but it displays COG profile details for COG profiles to which the user has access.
SELECT * FROM dbc.COGSTATUSV;
```

### DELETE COG profile

```sql
DROP COG PROFILE Research_Resources IN COG GROUP Research_Group;
```