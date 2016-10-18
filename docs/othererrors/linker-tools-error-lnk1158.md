---
title: "Linker Tools Error LNK1158"
ms.custom: na
ms.date: "10/14/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: na
ms.suite: na
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: na
ms.topic: "error-reference"
f1_keywords: 
  - "LNK1158"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "LNK1158"
ms.assetid: 45febf16-d9e1-42db-af91-532e2717fd6a
caps.latest.revision: 7
ms.author: "corob"
manager: "ghogen"
translation.priority.ht: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# Linker Tools Error LNK1158
cannot run 'filename'  
  
 The given executable file called by [LINK](../buildref/linker-command-line-syntax.md) is not in the directory that contains LINK nor in a directory specified in the PATH environment variable.  
  
 For example, you will get this error if you try to use the PGOPTIMIZE parameter to the [/LTCG](../buildref/-ltcg--link-time-code-generation-.md) linker option on a machine with a 32-bit operating system.