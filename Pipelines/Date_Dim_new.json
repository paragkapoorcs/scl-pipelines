{
    "name": "Date_Dimension_mod",
    "description": "Date Dimension",
    "artifact": {
        "name": "cdap-data-pipeline",
        "version": "6.4.1",
        "scope": "SYSTEM"
    },
    "config": {
        "resources": {
            "memoryMB": 2048,
            "virtualCores": 1
        },
        "driverResources": {
            "memoryMB": 2048,
            "virtualCores": 1
        },
        "connections": [
            {
                "from": "Drop and Create table",
                "to": "Create Date dimension"
            }
        ],
        "comments": [],
        "postActions": [],
        "properties": {},
        "processTimingEnabled": true,
        "stageLoggingEnabled": false,
        "stages": [
            {
                "name": "Drop and Create table",
                "plugin": {
                    "name": "BigQueryExecute",
                    "type": "action",
                    "label": "Drop and Create table",
                    "artifact": {
                        "name": "google-cloud",
                        "version": "0.17.3",
                        "scope": "SYSTEM"
                    },
                    "properties": {
                        "project": "auto-detect",
                        "dialect": "standard",
                        "mode": "batch",
                        "useCache": "false",
                        "location": "EU",
                        "rowAsArguments": "false",
                        "serviceAccountType": "filePath",
                        "serviceFilePath": "auto-detect",
                        "sql": "DROP TABLE IF EXISTS ${ProjectID}.${TargetDatasetName}.date_dimension;\nCREATE TABLE `${ProjectID}.${TargetDatasetName}.date_dimension`\n(\n  id STRING OPTIONS(description=\"Date in this the string fomat\"),\n  full_date DATE OPTIONS(description=\"full date in the YYYYMMDD format\"),\n  year INT64 OPTIONS(description=\"The year with century as a decimal number.\"),\n  month STRING OPTIONS(description=\"The month as a decimal number (01-12).\"),\n  week STRING OPTIONS(description=\"The week number of the year (Sunday as the first day of the week) as a decimal number (00-53).\"),\n  day STRING OPTIONS(description=\"The day as a decimal number (01-31)\"),\n  qtr STRING OPTIONS(description=\"The quarter as a decimal number (01-04).\"),\n  year_qtr STRING OPTIONS(description=\"concat year and quarter\"),\n  year_month STRING OPTIONS(description=\"concat year and month\"),\n  year_week STRING OPTIONS(description=\"concat year and week\"),\n  fiscal_year INT64 OPTIONS(description=\"fiscal year number\"),\n  fiscal_qtr STRING OPTIONS(description=\"fiscal quarter number\"),\n  month_name STRING OPTIONS(description=\"The full month name.\"),\n  week_day STRING OPTIONS(description=\"The weekday (Sunday as the first day of the week) as a decimal number (0-6).\"),\n  day_name STRING OPTIONS(description=\"The full weekday name.\"),\n  day_is_weekday INT64 OPTIONS(description=\"Weekday are marked as 1 and weekend as 0\")\n);"
                    }
                },
                "outputSchema": [
                    {
                        "name": "etlSchemaBody",
                        "schema": ""
                    }
                ],
                "id": "Drop-and-Create-table",
                "type": "action",
                "label": "Drop and Create table",
                "icon": "fa-plug"
            },
            {
                "name": "Create Date dimension",
                "plugin": {
                    "name": "BigQueryExecute",
                    "type": "action",
                    "label": "Create Date dimension",
                    "artifact": {
                        "name": "google-cloud",
                        "version": "0.17.3",
                        "scope": "SYSTEM"
                    },
                    "properties": {
                        "project": "${ProjectID}",
                        "sql": "SELECT\n  FORMAT_DATE('%F', d) as id,\n  d AS full_date,\n  EXTRACT(YEAR FROM d) AS year,\n  FORMAT_DATE('%m',d)  AS month,\n  FORMAT_DATE('%U', d) as week,\n  format(\"%02d\",EXTRACT(DAY FROM d)) AS day,\n  format(\"%02d\",CAST(FORMAT_DATE('%Q', d) AS INT64)) as qtr,\n  CAST(EXTRACT(YEAR FROM d) AS STRING) || CAST(format(\"%02d\",CAST(FORMAT_DATE('%Q', d) AS INT64)) AS STRING) as year_qtr,\n  CAST(EXTRACT(YEAR FROM d) AS STRING) || CAST(FORMAT_DATE('%m',d) AS STRING) AS year_month,\n  CAST(EXTRACT(YEAR FROM d) AS STRING) || CAST(FORMAT_DATE('%U', d) AS STRING) AS year_week,\n  EXTRACT(YEAR FROM d) AS fiscal_year,\n  format(\"%02d\",CAST(FORMAT_DATE('%Q', d) AS INT64)) as fiscal_qtr,\n  FORMAT_DATE('%B', d) as month_name,\n  FORMAT_DATE('%w', d) AS week_day,\n  FORMAT_DATE('%A', d) AS day_name,\n  (CASE WHEN FORMAT_DATE('%A', d) IN ('Sunday', 'Saturday') THEN 0 ELSE 1 END) AS day_is_weekday,\nFROM (\n  SELECT\n    *\n  FROM\n    UNNEST(GENERATE_DATE_ARRAY('2014-01-01', '2050-01-01', INTERVAL 1 DAY)) AS d )",
                        "dialect": "standard",
                        "mode": "batch",
                        "dataset": "scm_dataset1",
                        "table": "date_dimension",
                        "useCache": "false",
                        "location": "EU",
                        "rowAsArguments": "false",
                        "serviceAccountType": "filePath",
                        "serviceFilePath": "auto-detect"
                    }
                },
                "information": {
                    "comments": {
                        "list": [
                            {
                                "content": "Date dimension for getting the week and quarter for particular date",
                                "createDate": 1624353452840
                            }
                        ]
                    }
                },
                "outputSchema": [
                    {
                        "name": "etlSchemaBody",
                        "schema": ""
                    }
                ],
                "id": "Create-Date-dimension",
                "type": "action",
                "label": "Create Date dimension",
                "icon": "fa-plug"
            }
        ],
        "schedule": "0 * * * *",
        "engine": "spark",
        "numOfRecordsPreview": 100,
        "description": "Date Dimension",
        "maxConcurrentRuns": 1
    }
}
