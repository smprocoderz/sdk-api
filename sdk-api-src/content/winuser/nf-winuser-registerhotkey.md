---
UID: NF:winuser.RegisterHotKey
title: RegisterHotKey function (winuser.h)
description: Defines a system-wide hot key.
helpviewer_keywords: ["MOD_ALT","MOD_CONTROL","MOD_NOREPEAT","MOD_SHIFT","MOD_WIN","RegisterHotKey","RegisterHotKey function [Keyboard and Mouse Input]","_win32_RegisterHotKey","_win32_registerhotkey_cpp","inputdev.registerhotkey","winui._win32_registerhotkey","winuser/RegisterHotKey"]
old-location: inputdev\registerhotkey.htm
tech.root: inputdev
ms.assetid: VS|winui|~\winui\windowsuserinterface\userinput\keyboardinput\keyboardinputreference\keyboardinputfunctions\registerhotkey.htm
ms.date: 06/11/2024
ms.keywords: MOD_ALT, MOD_CONTROL, MOD_NOREPEAT, MOD_SHIFT, MOD_WIN, RegisterHotKey, RegisterHotKey function [Keyboard and Mouse Input], _win32_RegisterHotKey, _win32_registerhotkey_cpp, inputdev.registerhotkey, winui._win32_registerhotkey, winuser/RegisterHotKey
req.header: winuser.h
req.include-header: Windows.h
req.target-type: Windows
req.target-min-winverclnt: Windows Vista [desktop apps only]
req.target-min-winversvr: Windows Server 2003 [desktop apps only]
req.kmdf-ver: 
req.umdf-ver: 
req.ddi-compliance: 
req.unicode-ansi: 
req.idl: 
req.max-support: 
req.namespace: 
req.assembly: 
req.type-library: 
req.lib: User32.lib
req.dll: User32.dll
req.irql: 
targetos: Windows
req.typenames: 
req.redist: 
ms.custom: 19H1
f1_keywords:
 - RegisterHotKey
 - winuser/RegisterHotKey
dev_langs:
 - c++
topic_type:
 - APIRef
 - kbSyntax
api_type:
 - DllExport
api_location:
 - User32.dll
 - Ext-MS-Win-NTUser-Keyboard-l1-1-0.dll
 - Ext-MS-Win-NTUser-Keyboard-l1-1-1.dll
 - Ext-MS-Win-RTCore-NTUser-iam-l1-1-0.dll
 - ext-ms-win-ntuser-keyboard-l1-1-2.dll
 - Ext-MS-Win-NTUser-Keyboard-L1-2-0.dll
 - Ext-MS-Win-NTUser-Keyboard-L1-2-1.dll
 - Ext-MS-Win-RTCore-NTUser-Iam-L1-1-1.dll
 - Ext-MS-Win-NTUser-Keyboard-L1-3-0.dll
api_name:
 - RegisterHotKey
---

# RegisterHotKey function

## -description

Defines a system-wide hot key.

## -parameters

### -param hWnd [in, optional]

Type: **HWND**

A handle to the window that will receive <a href="/windows/desktop/inputdev/wm-hotkey">WM_HOTKEY</a> messages generated by the hot key. If this parameter is **NULL**, **WM_HOTKEY** messages are posted to the message queue of the calling thread and must be processed in the message loop.

### -param id [in]

Type: **int**

The identifier of the hot key.  If the *hWnd* parameter is NULL, then the hot key is associated with the current thread rather than with a particular window. If a hot key already exists with the same *hWnd* and *id* parameters, see Remarks for the action taken.

### -param fsModifiers [in]

Type: **UINT**

The keys that must be pressed in combination with the key specified by the *vk* parameter in order to generate the <a href="/windows/desktop/inputdev/wm-hotkey">WM_HOTKEY</a> message. The *fsModifiers* parameter can be a combination of the following values.

| **Value** | **Meaning** |
| --------- | ----------- |
| **MOD_ALT**<br>0x0001 | Either ALT key must be held down. |
| **MOD_CONTROL**<br>0x0002 | Either CTRL key must be held down. |
| **MOD_NOREPEAT**<br>0x4000 | Changes the hotkey behavior so that the keyboard auto-repeat does not yield multiple hotkey notifications.<br>**Windows Vista:** This flag is not supported. |
| **MOD_SHIFT**<br>0x0004 | Either SHIFT key must be held down. |
| **MOD_WIN**<br>0x0008 | Either WINDOWS key must be held down. These keys are labeled with the Windows logo. Keyboard shortcuts that involve the WINDOWS key are reserved for use by the operating system. |

### -param vk [in]

Type: **UINT**

The virtual-key code of the hot key. See <a href="/windows/desktop/inputdev/virtual-key-codes">Virtual Key Codes</a>.

## -returns

Type: **BOOL**

If the function succeeds, the return value is nonzero.

If the function fails, the return value is zero. To get extended error information, call <a href="/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror">GetLastError</a>.

This function fails if you try to associate a hot key with a window created by another thread.

Typically, **RegisterHotKey** also fails if the keystrokes specified for the hot key have already been registered for another hot key. However, some pre-existing, default hotkeys registered by the OS (such as PrintScreen, which launches the Snipping tool) may be overridden by another hot key registration when one of the app's windows is in the foreground.

## -remarks

When a key is pressed, the system looks for a match against all hot keys. Upon finding a match, the system posts the <a href="/windows/desktop/inputdev/wm-hotkey">WM_HOTKEY</a> message to the message queue of the window with which the hot key is associated. If the hot key is not associated with a window, then the **WM_HOTKEY** message is posted to the thread associated with the hot key.

If a hot key already exists with the same *hWnd* and *id* parameters, it is maintained along with the new hot key.  The application must explicitly call <a href="/windows/desktop/api/winuser/nf-winuser-unregisterhotkey">UnregisterHotKey</a> to unregister the old hot key.

The F12 key is reserved for use by the debugger at all times, so it should not be registered as a hot key. Even when you are not debugging an application, F12 is reserved in case a kernel-mode debugger or a just-in-time debugger is resident.

An application must specify an id value in the range 0x0000 through 0xBFFF. A shared DLL must specify a value in the range 0xC000 through 0xFFFF (the range returned by the <a href="/windows/desktop/api/winbase/nf-winbase-globaladdatoma">GlobalAddAtom</a> function). To avoid conflicts with hot-key identifiers defined by other shared DLLs, a DLL should use the **GlobalAddAtom** function to obtain the hot-key identifier.

**Windows Server 2003:  **If a hot key already exists with the same *hWnd* and *id* parameters, it is replaced by the new hot key.

### Examples

The following example shows how to use the **RegisterHotKey** function with the **MOD_NOREPEAT** flag.

In this example, the hotkey 'ALT+b' is registered for the main thread. When the hotkey is pressed, the thread will receive a <a href="/windows/desktop/inputdev/wm-hotkey">WM_HOTKEY</a> message, which will get picked up in the <a href="/windows/desktop/api/winuser/nf-winuser-getmessage">GetMessage</a> call. Because this example uses **MOD_ALT** with the **MOD_NOREPEAT** value for *fsModifiers*, the thread will only receive another **WM_HOTKEY** message when the 'b' key is released and then pressed again while the 'ALT' key is being pressed down.

```cpp
#include "stdafx.h"

int _cdecl _tmain (
    int argc, 
    TCHAR *argv[])
{           
    if (RegisterHotKey(
        NULL,
        1,
        MOD_ALT | MOD_NOREPEAT,
        0x42))  //0x42 is 'b'
    {
        _tprintf(_T("Hotkey 'ALT+b' registered, using MOD_NOREPEAT flag\n"));
    }
 
    MSG msg = {0};
    while (GetMessage(&msg, NULL, 0, 0) != 0)
    {
        if (msg.message == WM_HOTKEY)
        {
            _tprintf(_T("WM_HOTKEY received\n"));            
        }
    } 
 
    return 0;
}
```

## -see-also

<a href="/windows/desktop/api/winbase/nf-winbase-globaladdatoma">GlobalAddAtom</a>



<a href="/windows/desktop/inputdev/keyboard-input">Keyboard Input</a>



<a href="/samples/browse/?redirectedfrom=MSDN-samples">Register hotkey for the current app (CSRegisterHotkey)</a>



<a href="/samples/browse/?redirectedfrom=MSDN-samples">Register hotkey for the current app (CppRegisterHotkey)</a>



<a href="/samples/browse/?redirectedfrom=MSDN-samples">Register hotkey for the current app (VBRegisterHotkey)</a>





<a href="/windows/desktop/api/winuser/nf-winuser-unregisterhotkey">UnregisterHotKey</a>



<a href="/windows/desktop/inputdev/wm-hotkey">WM_HOTKEY</a>