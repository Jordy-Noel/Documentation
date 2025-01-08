# Database restore 

## Tier 2 To Tier 1

*Note | Make sure all services that can have a connection to your target database are stopped and any existing connections to the database are closed.*

1. In LCS navigate to your source environment.
2. Click on maintain -> Move database -> Export database.
    - This will create a database backup pacage in the asset library (.pacbac format).
3. Download the .bacpac file.
4. Open SQL Server management studio.
5. Right click on Databases
6. Import Data tier application
7. Follow the Wizard (duration 45 min)
8. Once the wizard is finished your newly created DB should be visible in the Database nodes.
9. Copy the destination database
10. Restore the new database onto the target database.
    - Create a bak file
    - REstore via that bak file (close existing connecitons)
11. Restart your AOS.