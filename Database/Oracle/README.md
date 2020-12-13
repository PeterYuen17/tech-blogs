# Oracle

## Scenario 1: Drop an admin user

``````
alter session set "_oracle_script" = true;
drop user username CASCADE;
``````