# Oracle

## Scenario 1: Drop all the tables of an admin user

The following command will not drop this user but all the tables it owns.

``````
alter session set "_oracle_script" = true;
drop user username CASCADE;
``````