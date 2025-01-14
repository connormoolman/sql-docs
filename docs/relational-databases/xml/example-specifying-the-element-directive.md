---
title: "Example: Specifying the ELEMENT Directive | Microsoft Docs"
description: View an example of how to specify the ELEMENT directive in an SQL query to generate element-centric XML.
ms.custom: ""
ms.date: "03/01/2017"
ms.prod: sql
ms.prod_service: "database-engine"
ms.reviewer: ""
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords: 
  - "ELEMENT directive"
ms.assetid: 80dd5d1f-fa90-4f97-a186-8fa3f460a7f3
author: MikeRayMSFT
ms.author: mikeray
---
# Example: Specifying the ELEMENT Directive
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  This retrieves employee information and generates element-centric XML as shown in the following:  
  
```  
<Employee EmpID=...>  
  <Name>  
    <FName>...</FName>  
    <LName>...</LName>  
  </Name>  
</Employee>  
```  
  
 The query remains the same, except you add the `ELEMENT` directive in the column names. Therefore, instead of attributes, the <`FName`> and <`LName`> element children are added to the <`Name`> element. Because the `Employee!1!EmpID` column does not specify the `ELEMENT` directive, `EmpID` is added as the attribute of the <`Employee`> element.  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID as [Employee!1!EmpID],  
       NULL       as [Name!2!FName!ELEMENT],  
       NULL       as [Name!2!LName!ELEMENT]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
ORDER BY [Employee!1!EmpID],[Name!2!FName!ELEMENT]  
FOR XML EXPLICIT;  
```  
  
 This is the partial result.  
  
 `<Employee EmpID="1">`  
  
 `<Name>`  
  
 `<FName>Ken</FName>`  
  
 `<LName>Sánchez</LName>`  
  
 `</Name>`  
  
 `</Employee>`  
  
 `<Employee EmpID="2">`  
  
 `<Name>`  
  
 `<FName>Terri</FName>`  
  
 `<LName>Duffy</LName>`  
  
 `</Name>`  
  
 `</Employee>`  
  
 `...`  
  
## See Also  
 [Use EXPLICIT Mode with FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  
