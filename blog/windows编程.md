### ssdt reset

```c
#inlcude <windows.h>
#pragma comment (lib, "ntdll.lib")
typedef struct _STRING {
	USHORT length;
	USHORT MaximumLength;
	PSTR Buffer;
}ANSI_STRING, *PANSI_STRING;

NTSTATUS WINAPI LdrGetProdureAddress(HMODULE ModuleHandle, PANSI_STRING FunctionName, WORD Oridinal, PVOID *FunctionAddress);
int main()
{
  PVOID ResetSSDT;
  ANSI_STRING str;
  str.Buffer = (PSTR)calloc(1, 0x40);
  str.MaximumLength = 0x40;
  str.length = 9;
  strcpy(str.Buffer, "ResetSSDT");
  LdrGetProcedureAddress((HMODULE)0x10000000, &str, 0, &ResetSSDT);
  return 0;
}
```

