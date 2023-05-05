# vxlang

<div align="center">
   <a href="https://vxlang.github.io/">
      <img src="https://vxlang.github.io/image/vxlang.gif" loop=infinite style="max-width: 100%; height: auto;" />
   </a>
</div>
<br>

- https://vxlang.github.io/

---

# Contents

- [What is vxlang?](#What-is-vxlang?)
- [Software Protector](#Software-Protector)
- [Software Code obfuscation and virtualization](#Software-Code-obfuscation-and-virtualization)
- [Virtualization Preview](#Virtualization-Preview)
- [Precautions](#Precautions)
- [VXLANG Extension Module](#VXLANG-Extension-Module)
- [Deploying the full version](#Deploying-the-full-version)
- [Latest Version](#Latest-Version)
- [TODO](#TODO)
- [Special Thanks](#Special-Thanks)

---

## What is vxlang?

Software can be described as human-understandable mnemonics through disassamblers, and additional information can be used to convert to advanced languages such as C/C++, which are easier to understand. However, this type of analysis can also be referred to as reverse engineering, which can pose a threat to the security of the software. 

**vxlang** is a project designed to prevent manipulations such as static or dynamic analysis, file modification, or unauthorized access by attackers, such as those described above. The vxlang project provides services for software security risks by implementing anti-tamper measures to prevent unauthorized access. 

The vxlang project currently targets native binary files on x86-64 systems and Microsoft Windows operating systems, including executable files with the ".exe" extension and dynamic link library files with the ".dll" extension, kernel driver files with the ".sys". (The target binary types supported by vxlang will be expanded in future updates.)

## Software Protector

<div align="center">
   <img src="https://vxlang.github.io/image/protector.png" loop=infinite style="max-width: 100%; height: auto;" />
</div>

Executable compression refers to the process of compressing a file into an executable format. This type of compression helps to hide source code and file information, making access more difficult. Software protectors such as vxlang can provide increased security by tampering with files, obfuscating code, and performing dynamic analysis protection to prevent unauthorized access or tampering.

## Software Code obfuscation and virtualization

<div align="center">
   <img src="https://vxlang.github.io/image/vcpu.png" loop=infinite style="max-width: 100%; height: auto;" />
</div>

The software protector effectively blocks access to encryption and runtime states, however, it has a drawback in that it can expose the source code when the runtime state is dumped. To address this issue, code obfuscation is applied by adding dummy code or modulating the source code with similar code to the code exposed in the static or dynamic state. However, a more effective solution to protect the code is Code-Virtualization. Code virtualization represents real-world operational commands as virtual code, which can be executed on internal virtual machines. This approach provides a higher level of security compared to code obfuscation alone. vxlang offers these advanced obfuscation and virtualization services to ensure the protection of the code.  

## Virtualization Preview

<div align="center">
   <p>Before</p>
   <img src="https://vxlang.github.io/image/VMBegin.png" loop=infinite style="max-width: 100%; height: auto;" />
   <p>After</p>
   <img src="https://vxlang.github.io/image/VMEnd.png" loop=infinite style="max-width: 100%; height: auto;" />
   <p>Run</p>   
   <img src="https://vxlang.github.io/image/VMRun.gif" loop=infinite style="max-width: 100%; height: auto;" />
</div>

## Precautions

- `Optimization` : Verify that VxVirtualizationBegin and VxVirtualizationEnd are in the same block.  
  If procedures are merged due to code optimization, problems may arise.
- `switch-case` : In the switch-case, when the branch movement is not an IMM value, it may be moved to the original position.
- `Exception handling` : Currently, the virtual CPU does not support exception handling.

## VXLANG Extension Module

- Example
  ```cpp
  #include <windows.h>
  #include <stdio.h>
  
  #define VXLANG_ADDON_MODULE
  #include "vxlib.h"
  
  BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpvReserved) {
      BOOL result = TRUE;
  
      switch (fdwReason) {
      case DLL_PROCESS_ATTACH:
          break;
      case DLL_THREAD_ATTACH:
          break;
      case VXLANG_LOAD_ADDON_EVENT:
          break;
      case VXLANG_DETECTED_DEBUG:
      case VXLANG_DETECTED_PATCH:
          break;
      case VXLANG_START_EVENT:
          break;
      case VXLANG_TERMINATE_EVENT:
          break;
      default:
          break;
      }
  
      return result;
  }
  
  /**
   * 
   */
  
  void NTAPI TlsCallback1(PVOID DllHandle, DWORD dwReason, PVOID) {
      if (dwReason == DLL_PROCESS_ATTACH) {
      }
      else if (dwReason == DLL_THREAD_ATTACH) {
      }
  }
  
  void NTAPI TlsCallback2(PVOID DllHandle, DWORD dwReason, PVOID) {
      if (dwReason == DLL_PROCESS_ATTACH) {
      }
      else if (dwReason == DLL_THREAD_ATTACH) {
      }
  }
  
  #ifdef _WIN64
  #pragma comment (linker, "/INCLUDE:_tls_used") 
  #pragma comment (linker, "/INCLUDE:_tls_callback_list")
  #else
  #pragma comment (linker, "/INCLUDE:__tls_used") 
  #pragma comment (linker, "/INCLUDE:__tls_callback_list")
  #endif
  
  #ifdef _WIN64
  #pragma const_seg(".CRT$XLC")
  EXTERN_C const
  #else
  #pragma data_seg(".CRT$XLC")
  EXTERN_C
  #endif
  PIMAGE_TLS_CALLBACK _tls_callback_list[] = { TlsCallback1, TlsCallback2 };
  #ifdef _WIN64
  #pragma const_seg()
  #else
  #pragma data_seg()
  #endif 
  ```
  ```
  vxlang.exe ${target-path} --add-on ${add-on-path}
  ```
  - Warning
    - If DllMain's return value is `FALSE`, **VXLANG_TERMINATE_EVENT** is called.
  ```cpp
	BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpvReserved) {
		BOOL result = TRUE;

		switch (fdwReason) {
		case DLL_PROCESS_ATTACH:
			break;
		case DLL_THREAD_ATTACH:
			break;
		case VXLANG_LOAD_ADDON_EVENT:
			printf("VXLANG_LOAD_ADDON_EVENT \n");
			result = FALSE;
			break;
		case VXLANG_DETECTED_DEBUG:
		case VXLANG_DETECTED_PATCH:
		case VXLANG_DETECTED_PATCH_SHELL:
		case VXLANG_DETECTED_PATCH_IMAGE:
		case VXLANG_DETECTED_PAUSE:
		case VXLANG_DETECTED_HANDLE:
		case VXLANG_DETECTED_SHELL:
		case VXLANG_DETECTED_DLL:
			break;
		case VXLANG_TERMINATE_EVENT:
			printf("VXLANG_TERMINATE_EVENT \n");
			break;
		default:
			break;
		}
		return result;
	}  
  ```
  
## Deploying the full version

The beta version of vxlang is free software, please request the full version via email and we will respond by creating your distribution file.
- Support for specialized add-on modules.
- Support for virtual CPUs with specialized CPU contexts.

## Latest Version

0.9.3
---
- Detailed output of the progress of obfuscation, virtualization, and packing.
- Part of the Detect routine has been removed.
  - It will be distributed via an extension module in the full version.
- Add the x86-64 virtualization command.
- Fixed a bug.
- [Download](https://github.com/vxlang/vxlang-page/)

## TODO

- `0.9.4`
  - Supports packing for .NET executables.
  -  Add the x86-64 virtualization command.
- `Task`
  - Add ELF32/64 format for x86-64
  - Add code obfuscation methods for x86-64
    - Once the above steps are complete, add the ARM system for the ELF file format.
    
---

## Special Thanks

Thank you to everyone who helped with the development.

- [2GG](https://twitter.com/2gg) 
- [kao](https://lifeinhex.com/) 
  - [Crackme01](https://forum.tuts4you.com/topic/43809-users-desktop-crackme/#comment-213340) 
- [Extreme Coders](https://github.com/extremecoders-re/tuts4you_users_desktop_crackme_writeup) 
  - [Crackme01](https://forum.tuts4you.com/topic/43809-users-desktop-crackme/#comment-213328)  
- [clavis0x](https://github.com/clavis0x)
- [tybins99](https://github.com/tybins99)  
  
  
  
