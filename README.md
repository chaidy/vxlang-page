# VxLang

<div align="center">
   <a href="https://vxlang.github.io/">
      <img src="https://vxlang.github.io/image/vxlang.gif" loop=infinite style="max-width: 100%; height: auto;" />
   </a>
</div>
<br>

- [BLOG](https://vxlang.github.io/)

---

## What is vxlang?

**VXLANG** is a project designed to prevent reverse-engineering behaviors such as static or dynamic analysis, file tampering, and unauthorized access to memory by attackers. 

The vxlang project currently targets x86-64 system and .Net binaries, native binary files for the Microsoft Windows operating system, including executables with the ".exe" extension, dynamic link library files with the ".dll" extension, and kernel driver files with the ".sys" extension. (The types of target binaries supported by vxlang will be expanded in future updates).

## Virtualization Preview

<div align="center">
   <p>Before</p>
   <img src="https://vxlang.github.io/image/VMBegin.png" style="max-width: 100%; height: auto;" />
   <p>After</p>
   <img src="https://vxlang.github.io/image/VMEnd.png" style="max-width: 100%; height: auto;" />
</div>

## Obfuscation Preview

<div align="center">
   <p>Befor</p>
   <img src="https://vxlang.github.io/image/bef.PNG" style="max-width: 100%; height: auto;" />
   <p>After</p>
   <img src="https://vxlang.github.io/image/aft.PNG" style="max-width: 100%; height: auto;" /> 
   <img src="https://vxlang.github.io/image/aft2.PNG" style="max-width: 100%; height: auto;" /> 
</div>

## Precautions

- `Optimization` : Verify that VxVirtualizationBegin and VxVirtualizationEnd are in the same block.  
  If procedures are merged due to code optimization, problems may arise.
- `switch-case` : In the switch-case, when the branch movement is not an IMM value, it may be moved to the original position.
- `Exception handling` : Currently, the virtual CPU does not support exception handling.
  
## Deploying the full version

The full version of the VxLang beta is free software, so if you share with me what you'll be using it for, I'll share a download link with you.

- E-Mail: 0x999h@gmail.com
- The demo message box is removed.
- Support for specialized add-on modules.
- Support for virtual CPUs with specialized CPU contexts.
- Supports on/off for packer and engine code.
- Add-on development allows you to develop additional features.
- Anti-debugging and anti-tamper features are adjustable.
- Users can disable or control error messages.

## Latest Version

0.9.7
---
  - 20230802
    - Consolidate the obfuscation section.(.vxil0~ vxil1)
      - Added a form of obfuscation.
  - 20230730
    - Packers and obfuscators are also modified on a per-platform basis.
    - Obfuscation methods (e.g. control-flow-flattening, CFF) are added. Adds x86-64 assembler to the obfuscator, and updates it to make
      - **VL_CODE_FLATTENING_BEGIN** / **VL_CODE_FLATTENING_END**  
  - [Download](https://vxlang.github.io/download.html)
      
## TODO
- `Hotfix`
  - There is a small bug in the obfuscation and flattening nesting process.
  - I found an intermittent problem with vxlang's flattening process.
  - Change the way virtual machines are created.
    - Replaces the vxlang virtual machine with the vxlang internal x86-64 assembler.
  - The packer's default protection is restored. This works regardless of the AXION module.
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
- [EBalloon](https://github.com/EBalloon)
  
  
  
