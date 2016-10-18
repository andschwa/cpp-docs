---
title: "How to: Call Native DLLs from Managed Code Using PInvoke"
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
ms.assetid: 3273eb4b-38d1-4619-92a6-71bda542be72
caps.latest.revision: 18
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
# How to: Call Native DLLs from Managed Code Using PInvoke
Functions that are implemented in unmanaged DLLs can be called from managed code using Platform Invoke (P/Invoke) functionality. If the source code for the DLL is not available, P/Invoke is the only option for interoperating. However, unlike other .NET languages, Visual C++ provides an alternative to P/Invoke. For more information, see [Using C++ Interop (Implicit PInvoke)](../VS_visualcpp/Using-C---Interop--Implicit-PInvoke-.md).  
  
## Example  
 The following code example uses the Win32 [GetSystemMetrics](http://msdn.microsoft.com/library/windows/desktop/ms724385) function to retrieve the current resolution of the screen in pixels.  
  
 For functions that use only intrinsic types as arguments and return values, no extra work is required. Other data types, such as function pointers, arrays, and structures, require additional attributes to ensure proper data marshaling.  
  
 Although it is not required, it is good practice to make P/Invoke declarations static members of a value class so that they do not exist in the global namespace, as demonstrated in this example.  
  
```  
// pinvoke_basic.cpp  
// compile with: /clr  
using namespace System;  
using namespace System::Runtime::InteropServices;  
  
value class Win32 {  
public:  
   [DllImport("User32.dll")]  
   static int GetSystemMetrics(int);  
  
   enum class SystemMetricIndex {  
      // Same values as those defined in winuser.h.  
      SM_CXSCREEN = 0,  
      SM_CYSCREEN = 1  
   };  
};  
  
int main() {  
   int hRes = Win32::GetSystemMetrics( safe_cast<int>(Win32::SystemMetricIndex::SM_CXSCREEN) );  
   int vRes = Win32::GetSystemMetrics( safe_cast<int>(Win32::SystemMetricIndex::SM_CYSCREEN) );  
   Console::WriteLine("screen resolution: {0},{1}", hRes, vRes);  
}  
```  
  
## See Also  
 [Using Explicit PInvoke in C++ (DllImport Attribute)](../VS_visualcpp/Using-Explicit-PInvoke-in-C----DllImport-Attribute-.md)