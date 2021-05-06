## ETL Training  Exercises Solution 
### Prep work
I created SQL Server instance and restored backed up databases:
 - STAGE_AdventureWorks2017
 - CLEANSE_AdventureWorks2017
 - CORE_AdventureWorks2017
 - DATAMART_AdventureWorks2017

### Exercise 1
Zadanie File to Stage - porównaj dane pomiędzy tabelami z folderu ‘source_files’ oraz tabelami z bazy STAGE. Tabele [HumanResources].[Department], [HumanResources].[Shift] oraz [Person].[AddressType] porównaj za pomocą odpowiednich formuł w Excel, pozostałe zaimportuj do bazy STAGE i porównaj pisząc odpowiednie zapytania.
#### Solution - SSMS part
##### Create tables based on csv files
Using `BULK INSERT` I imported data from csv files to the `STAGE_AdventureWorks2017` database. I used `Csv` suffix for new tables. 
[Used SQL queries](https://drive.google.com/file/d/1JBz0-Fxj_ZNwOqv1YRpJEIJpYuS7YM_3/view?usp=sharing)
##### Transform tables
Next, I transformed `Csv` tables (and in some cases original tables) so they match original `STAGE_AdventureWorks2017`tables. 
##### Compare data
Then I execute `EXCEPT` on selects of these tables.
[Used SQL queries and detailed results](https://drive.google.com/file/d/19eQUrg6o7hxtcgP_v1MSFYygWzyrtC5k/view?usp=sharing)
##### Results
***Almost all*** tables are identical to csv files. 
#### Solution - Excel part
##### Export tables to csv files
To check `HumanResources].[Department]`, `[HumanResources].[Shift]` and `[Person].[AddressType]` tables  in Excel I exported these tables from `STAGE_AdventureWorks2017` to csv files using `bcp` command in `cmd`.
[Used cmd command](https://drive.google.com/file/d/1AqtIfv890-LOfmCh9pCtpoRDzmvx1LBK/view?usp=sharing)
##### Import tables to Excel
Next, I imported csv files to Excel. I decided to import them as a single line, because provided csv files were like that.
##### Compare data
I decided to base my comparing formula on `VLOOKUP`. I executed it on the data.
##### Results
The data in provided csv files is ***identical*** to the data exported from the database by me.
[Excel file](https://drive.google.com/file/d/1-VkG1QWgbPeL0jNPBZ5hto77XlRDCqTk/view?usp=sharing)
### Exercise 2
Zadanie Stage to Cleans – Porównaj strukturę danych oraz dane pomiędzy tabelami według dokumentacji znajdującej się w folderze ‘Stage to Cleans mappings’. Uwzględnij transformacje w zapytaniach, jeśli to konieczne. Do wykonania zadania pomocne będzie korzystanie z pliku ‘Data Structure’. Zapisz query oraz zrzuty ekranu z bazy z wynikami zapytania.
#### Solution
##### Compare data types and the data
I campared data types of the columns from `[STAGE_AdventureWorks2017]` and `[CLEANSE_AdventureWorks2017]` using `EXCEPT` both ways. Columns transformations were needed in many cases.
##### Results
In ***most cases*** data types and the data are identical. In all cases `[ModifiedDate]` has different values. There's only one table that has more columns in one databasa compared to the other (`[HumanResources].[Employee]`).
[Used SQL queries and detailed results](https://drive.google.com/file/d/1w7g-J6FyTcuwwjf2E66vVT7FGJ4k44vB/view?usp=sharing)
### Exercise 3
Zadanie Cleans to Core - Porównaj strukturę danych oraz dane pomiędzy tabelami według dokumentacji znajdującej się w folderze ‘Cleans to Core mappings’. Uwzględnij transformacje w zapytaniach, jeśli to konieczne. Do wykonania zadania pomocne będzie korzystanie z pliku ‘Data Structure’. Zapisz query oraz zrzuty ekranu z bazy z wynikami zapytania.
#### Solution
##### Compare data types and the data
I campared data types of the columns from `[CLEANSE_AdventureWorks2017]` and `[CORE_AdventureWorks2017]` using `EXCEPT` both ways. Columns transformations were needed in many cases.
##### Results
In ***all cases*** data types are the same. In ***most cases*** the data is identical. In some cases there are NULLs in one table where there are empty string in the other. Also, `[Revision]` column can have different values.
[Used SQL queries and detailed results](https://drive.google.com/file/d/1225ZRuN0BdCbv-s4WLymOojJw6I0yBuH/view?usp=sharing)
### Exercise 4
Zadanie Core to DataMart - Porównaj strukturę danych oraz dane pomiędzy tabelami według dokumentacji znajdującej się w folderze ‘Core to DataMart mappings’ . Diagramy z folderu ‘ETL Diagrams’ mają charakter pomocniczy wskazujący na funkcje, których należy użyć podczas pisania zapytań SQL. Uwzględnij transformacje w zapytaniach przedstawione w folderze ‘Data flows’ (transformacje dla poszczególnych tabel) . Do wykonania zadania pomocne będzie korzystanie z pliku ‘Data Structure’. Zapisz query oraz zrzuty ekranu z bazy z wynikami zapytania.
#### Solution
##### Compare data types and the data
I campared data types of the columns from `[CORE_AdventureWorks2017]` and `[DATAMART_AdventureWorks2017]` using `EXCEPT` both ways. Heavy columns transformations were needed in most cases, especially when `xml`-type columns were involved.
##### Results
Data types in ***most cases***  were identical, except for some mostly minor differences. Data was the same in ***most cases*** as well. In few cases there were discrepancies regarding number of records (`[DATAMART_AdventureWorks2017].[dbo].[JobCandidate]` and `[DATAMART_AdventureWorks2017].[dbo].[SalesPerson]`) or the size of the field (`[DATAMART_AdventureWorks2017].[dbo].[JobCandidateEducation]`).
Also, there were some discrepancies between models (both diagrams and tables) and the tables.
I had problems (regrettably, I didn't manage to overcome them) to fully recreate transformations for one table, namely `[DATAMART_AdventureWorks2017].[dbo].[AdditionalContactInfo]`.
[Used SQL queries and detailed results](https://drive.google.com/file/d/1QPKaUv1iITIREVe9qA1YUVDOEI8ni3sW/view?usp=sharing)
