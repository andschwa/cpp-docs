---
title: "CConstraintTableUsage, CConstraintTableUsageInfo | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: ["cpp-windows"]
ms.tgt_pltfrm: ""
ms.topic: "reference"
f1_keywords: ["CConstraintTableUsageInfo", "CONSTRAINT_TABLE_USAGE", "m_szTableSchema", "m_szConstraintCatalog", "CONSTRAINT_NAME", "m_szTableCatalog", "m_szConstraintSchema", "m_szTableName", "CONSTRAINT_CATALOG", "CONSTRAINT_SCHEMA", "CConstraintTableUsage", "m_szConstraintName"]
dev_langs: ["C++"]
helpviewer_keywords: ["CConstraintTableUsage typedef class", "m_szConstraintCatalog", "CONSTRAINT_CATALOG", "m_szTableSchema", "CConstraintTableUsageInfo parameter class", "TABLE_CATALOG", "CONSTRAINT_TABLE_USAGE", "TABLE_NAME", "CONSTRAINT_NAME", "CONSTRAINT_SCHEMA", "TABLE_SCHEMA", "m_szTableCatalog", "m_szConstraintName", "m_szTableName", "m_szConstraintSchema"]
ms.assetid: 666b44de-3922-4c5e-ad17-d5ea27120174
caps.latest.revision: 6
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
ms.workload: ["cplusplus", "data-storage"]
---
# CConstraintTableUsage, CConstraintTableUsageInfo
Call the typedef class **CConstraintTableUsage** to implement its parameter class **CConstraintTableUsageInfo**.  
  
## Remarks  
 See [Schema Rowset Classes and Typedef Classes](../../data/oledb/schema-rowset-classes-and-typedef-classes.md) for more information on using typedef classes.  
  
 This class identifies the tables used by referential constraints, unique constraints, check constraints, and assertions, defined in the catalog and owned by a given user.  
  
 The following table lists the class data members and their corresponding OLE DB Columns. See [CONSTRAINT_TABLE_USAGE Rowset](https://msdn.microsoft.com/en-us/library/ms724522.aspx) in the *OLE DB Programmer's Reference* for more information about the schema and columns.  
  
|Data members|OLE DB columns|  
|------------------|--------------------|  
|m_szTableCatalog|TABLE_CATALOG|  
|m_szTableSchema|TABLE_SCHEMA|  
|m_szTableName|TABLE_NAME|  
|m_szConstraintCatalog|CONSTRAINT_CATALOG|  
|m_szConstraintSchema|CONSTRAINT_SCHEMA|  
|m_szConstraintName|CONSTRAINT_NAME|  
  
## Requirements  
 **Header:** atldbsch.h  
  
## See Also  
 [CRestrictions Class](../../data/oledb/crestrictions-class.md)