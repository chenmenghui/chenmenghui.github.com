---
title: mysql事务之修改不定的表结构
date: 2019-12-19
categories: 
- MySQL
tags: 
- MySQL
---

```sql
DELIMITER ;;
DROP PROCEDURE IF EXISTS Alter_Table_Valen;;
CREATE PROCEDURE Alter_Table_Valen()
BEGIN
    DECLARE ColumnExists int;
    DECLARE TableName varchar(50);
    DECLARE ColumnName varchar(50);
    DECLARE AddColumnSql text;
    SET ColumnExists = 0;
    SET TableName = 'maintenanceschedule_set';
    SET ColumnName = 'VisitCount';
    SET AddColumnSql = ' ADD (`OverlapRule` tinyint(1) NOT NULL DEFAULT 0,
        `VisitCount` int(2) NOT NULL DEFAULT 0,
        `ScheduleMethod` tinyint(2) NOT NULL DEFAULT 0,
        `MethodOption` varchar(255) NOT NULL DEFAULT '''',
        `OtherLogic` varchar(255) NOT NULL DEFAULT '''',
        `Priority` int(2) NOT NULL DEFAULT 0
        )';
    SELECT count(*)
    INTO ColumnExists
    FROM information_schema.COLUMNS
    WHERE TABLE_NAME = TableName
      AND COLUMN_NAME = ColumnName;
    IF (ColumnExists = 0) THEN
        SET @SQL1 = concat('ALTER TABLE ', TableName, AddColumnSql);
        PREPARE stmt1 FROM @SQL1;
        EXECUTE stmt1;
    END IF;
END ;;
CALL Alter_Table_Valen;;
DROP PROCEDURE IF EXISTS Alter_Table_Valen;;
DELIMITER ;
```

