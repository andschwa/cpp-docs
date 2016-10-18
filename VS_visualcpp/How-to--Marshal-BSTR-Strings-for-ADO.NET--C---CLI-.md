---
title: "How to: Marshal BSTR Strings for ADO.NET (C++-CLI)"
ms.custom: na
ms.date: 10/03/2016
ms.devlang: 
  - C++
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - devlang-cpp
ms.tgt_pltfrm: na
ms.topic: get-started-article
H1: How to: Marshal BSTR Strings for ADO.NET (C++/CLI)
ms.assetid: 5daf4d9e-6ae8-4604-908f-855e37c8d636
caps.latest.revision: 9
manager: ghogen
translation.priority.ht: 
  - cs-cz
  - de-de
  - es-es
  - fr-fr
  - it-it
  - ja-jp
  - ko-kr
  - pl-pl
  - pt-br
  - ru-ru
  - tr-tr
  - zh-cn
  - zh-tw
---
# How to: Marshal BSTR Strings for ADO.NET (C++-CLI)
Demonstrates how to add a COM string (`BSTR`) to a database and how to marshal a <xref:System.String?qualifyHint=True> from a database to a `BSTR`.  
  
## Example  
 In this example, the class DatabaseClass is created to interact with an ADO.NET <xref:System.Data.DataTable?qualifyHint=False> object. Note that this class is a native C++ `class` (as compared to a `ref class` or `value class`). This is necessary because we want to use this class from native code, and you cannot use managed types in native code. This class will be compiled to target the CLR, as is indicated by the `#pragma managed` directive preceding the class declaration. For more information on this directive, see [managed, unmanaged](../VS_visualcpp/managed--unmanaged.md).  
  
 Note the private member of the DatabaseClass class: `gcroot<DataTable ^> table`. Since native types cannot contain managed types, the `gcroot` keyword is necessary. For more information on `gcroot`, see [How to: Declare Handles in Native Types](../VS_visualcpp/How-to--Declare-Handles-in-Native-Types.md).  
  
 The rest of the code in this example is native C++ code, as is indicated by the `#pragma unmanaged` directive preceding `main`. In this example, we are creating a new instance of DatabaseClass and calling its methods to create a table and populate some rows in the table. Note that COM strings are being passed as values for the database column StringCol. Inside DatabaseClass, these strings are marshaled to managed strings using the marshaling functionality found in the <xref:System.Runtime.InteropServices?qualifyHint=True> namespace. Specifically, the method <xref:System.Runtime.InteropServices.Marshal.PtrToStringBSTR?qualifyHint=False> is used to marshal a `BSTR` to a <xref:System.String?qualifyHint=False>, and the method <xref:System.Runtime.InteropServices.Marshal.StringToBSTR?qualifyHint=False> is used to marshal a <xref:System.String?qualifyHint=False> to a `BSTR`.  
  
> [!NOTE]
>  The memory allocated by <xref:System.Runtime.InteropServices.Marshal.StringToBSTR?qualifyHint=False> must be deallocated by calling either <xref:System.Runtime.InteropServices.Marshal.FreeBSTR?qualifyHint=False> or `SysFreeString`.  
  
```  
// adonet_marshal_string_bstr.cpp  
// compile with: /clr /FU System.dll /FU System.Data.dll /FU System.Xml.dll  
#include <comdef.h>  
#include <gcroot.h>  
#include <iostream>  
using namespace std;  
  
#using <System.Data.dll>  
using namespace System;  
using namespace System::Data;  
using namespace System::Runtime::InteropServices;  
  
#define MAXCOLS 100  
  
#pragma managed  
class DatabaseClass  
{  
public:  
    DatabaseClass() : table(nullptr) { }  
  
    void AddRow(BSTR stringColValue)  
    {  
        // Add a row to the table.  
        DataRow ^row = table->NewRow();  
        row["StringCol"] = Marshal::PtrToStringBSTR(  
            (IntPtr)stringColValue);  
        table->Rows->Add(row);  
    }  
  
    void CreateAndPopulateTable()  
    {  
        // Create a simple DataTable.  
        table = gcnew DataTable("SampleTable");  
  
        // Add a column of type String to the table.  
        DataColumn ^column1 = gcnew DataColumn("StringCol",  
            Type::GetType("System.String"));  
        table->Columns->Add(column1);  
    }  
  
    int GetValuesForColumn(BSTR dataColumn, BSTR *values,  
        int valuesLength)  
    {  
        // Marshal the name of the column to a managed  
        // String.  
        String ^columnStr = Marshal::PtrToStringBSTR(  
                (IntPtr)dataColumn);  
  
        // Get all rows in the table.  
        array<DataRow ^> ^rows = table->Select();  
        int len = rows->Length;  
        len = (len > valuesLength) ? valuesLength : len;  
        for (int i = 0; i < len; i++)  
        {  
            // Marshal each column value from a managed string  
            // to a BSTR.  
            values[i] = (BSTR)Marshal::StringToBSTR(  
                (String ^)rows[i][columnStr]).ToPointer();  
        }  
  
        return len;  
    }  
  
private:  
    // Using gcroot, you can use a managed type in  
    // a native class.  
    gcroot<DataTable ^> table;  
};  
  
#pragma unmanaged  
int main()  
{  
    // Create a table and add a few rows to it.  
    DatabaseClass *db = new DatabaseClass();  
    db->CreateAndPopulateTable();  
  
    BSTR str1 = SysAllocString(L"This is string 1.");  
    db->AddRow(str1);  
  
    BSTR str2 = SysAllocString(L"This is string 2.");  
    db->AddRow(str2);  
  
    // Now retrieve the rows and display their contents.  
    BSTR values[MAXCOLS];  
    BSTR str3 = SysAllocString(L"StringCol");  
    int len = db->GetValuesForColumn(  
        str3, values, MAXCOLS);  
    for (int i = 0; i < len; i++)  
    {  
        wcout << "StringCol: " << values[i] << endl;  
  
        // Deallocate the memory allocated using  
        // Marshal::StringToBSTR.  
        SysFreeString(values[i]);  
    }  
  
    SysFreeString(str1);  
    SysFreeString(str2);  
    SysFreeString(str3);  
    delete db;  
  
    return 0;  
}  
```  
  
 **StringCol: This is string 1.**  
**StringCol: This is string 2.**   
## Compiling the Code  
  
-   To compile the code from the command line, save the code example in a file named adonet_marshal_string_native.cpp and enter the following statement:  
  
    ```  
    cl /clr /FU System.dll /FU System.Data.dll /FU System.Xml.dll adonet_marshal_string_native.cpp  
    ```  
  
## .NET Framework Security  
 For information on security issues involving ADO.NET, see [Securing ADO.NET Applications](../Topic/Securing%20ADO.NET%20Applications.md).  
  
## See Also  
 <xref:System.Runtime.InteropServices?qualifyHint=False>   
 [Data Access Using ADO.NET (C++/CLI)](../VS_visualcpp/Data-Access-Using-ADO.NET--C---CLI-.md)   
 [ADO.NET](../Topic/ADO.NET.md)   
 [Interoperability](assetId:///afcc2e7d-3f32-48d2-8141-1c42acf29084)   
 [Native and .NET Interoperability](../VS_visualcpp/Native-and-.NET-Interoperability.md)