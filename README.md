# testsql
Find the names of all customers with a shipping address
SELECT FirstName , MiddleName , LastName
FROM Person.Person pp
JOIN Sales. Customer sc
ON pp. BusinessEntityID=sc.PersonID
JOIN Sales. SalesOrderHeader  sso
ON sso. BusinessEntityID=pp. BusinessEntityID
JOIN Sales.Address sa
ON sa.AddressID= sso.ShipToAddressID 
WHERE sa.AddressLine1 is not null 
OR sa.AddressLine2 is not null

Find the number of bicycle stores in Victoria Australia (Note: ALL stores are bicycle stores)
SELECT count(*)
FROM Sales .Store ss
JOIN Person.BusinessEntityAddress  pb
ON ss. BusinessEntityID =pb. BusinessEntityID 
JOIN Person.Address pa
ON pb. AddressID =pa. AddressID 
JOIN Person.StateProvince ps
ON ps. StateProvinceID=pa. StateProvinceID 
Where ps. StateProvinceID =” Victoria” 

Find the names of all current employees who are paid monthly
SELECT FirstName , MiddleName , LastName 
FROM Person.Person pp
JOIN  HumanResources.Employee he
ON  pp. BusinessEntityID = he. BusinessEntityID 
JOIN HumanResources. EmployeePayHistory  hep
ON he. BusinessEntityID=hep. BusinessEntityID
WHERE hep. PayFrequency=” monthly”


Find the names of all current employees that work in departments other than the sales department.
SELECT FirstName , MiddleName , LastName 
FROM Person.Person pp
JOIN  HumanResources.Employee he
ON  pp. BusinessEntityID = he. BusinessEntityID )
JOIN HumanResources. EmployeeDepartmentHistory hed
ON he. BusinessEntityID =hed. BusinessEntityID 
JOIN HumanResources .Department hd
ON hed. DepartmentID = he. DepartmentID 
WHERE hd. Name <> “sales department”
 

Find the names of bicycle stores in Victoria Australia (Note: ALL stores in the database are bicycle stores)
SELECT ss. Name 
FROM Sales .Store ss
JOIN Person.BusinessEntityAddress  pb
ON ss. BusinessEntityID =pb. BusinessEntityID 
JOIN Person.Address pa
ON pb. AddressID =pa. AddressID 
JOIN Person.StateProvince ps
ON ps. StateProvinceID=pa. StateProvinceID 
Where ps. Name =” Victoria” 



Find the total number of bicycle stores in each state of Australia associated with this company (Note: ALL stores are "bicycle" stores in the database)
SELECT ps.Name,Count(*) 
FROM Sales .Store ss
JOIN Person.BusinessEntityAddress  pb
ON ss. BusinessEntityID =pb. BusinessEntityID 
JOIN Person.Address pa
ON pb. AddressID =pa. AddressID 
JOIN Person.StateProvince ps
ON ps. StateProvinceID=pa. StateProvinceID 
GROUP BY ps.Name 

Find the name, age and current department of the oldest person still working for the company in the database
SELECT FirstName , MiddleName , LastName, 
DATEDIFF ( year, ha.StartTime , hs.EndTime),Hd.Name
FROM Person.Person pp
JOIN  HumanResources.Employee he
ON  pp. BusinessEntityID = he. BusinessEntityID 
JOIN HumanResources .EmployeeDepartmentHistory hedh
ON hedh. BusinessEntityID=he. BusinessEntityID
JOIN HumanResources .Shift hs 
ON hs. ShiftID =hedh. ShiftID 
JOUN HumanResources. Department hd
ON hd. DepartmentID=hedh. DepartmentID 
ORDER BY DATEDIFF( day, ha.StartTime , hs.EndTime) DESC
WHERE AND ROWNUM = 1;






Find the number of Job Candidates for each department including those whose department cannot be determined.  Use the words “Unknown” for candidates where the department cannot be determined.
SELECT
(case WHEN hd.Name is null THEN “Unknown”ELSE hd.Name END),
hd. Name,Count(*)
FROM HumanResources .JobCandidate hj
JOIN HumanResources. Employee he
ON he. BusinessEntityID = hj. BusinessEntityID
JOIN HumanResources.EmployeeDepartmentHistory  hedh
ON he. BusinessEntityID = hedh. BusinessEntityID 
JOIN HumanResources .Department hd
ON hedh. DepartmentID=hd. DepartmentID
GOUP BY hd. Name 
