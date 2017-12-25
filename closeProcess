#pragma comment( linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"" ) // 设置入口地址

#include"stdio.h"
#include <iostream>
#include <windows.h>
#include <tlhelp32.h> 

using namespace std;

void shutdown()
{
	// 以下为提权函数，使其用关机权限,
	HANDLE hToken;
	TOKEN_PRIVILEGES tkp;

	if (!OpenProcessToken(GetCurrentProcess(), TOKEN_ADJUST_PRIVILEGES| TOKEN_QUERY, &hToken))
	{
		printf("OpenProcessToken调用失败");
	}
	LookupPrivilegeValue(NULL, SE_SHUTDOWN_NAME, &tkp.Privileges[0].Luid);
	tkp.PrivilegeCount = 1;
	tkp.Privileges[0].Attributes = SE_PRIVILEGE_ENABLED;
	AdjustTokenPrivileges(hToken, false, &tkp, 0,(PTOKEN_PRIVILEGES)NULL, 0);

	//下面为关机函数
	if (!ExitWindowsEx(EWX_SHUTDOWN, 0))
	{
		printf("关机失败");
	}
}

void checkProcess() {
	PROCESSENTRY32 pe32;
	pe32.dwSize = sizeof(pe32);

	HANDLE hProcessSnap = ::CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
	if (hProcessSnap == INVALID_HANDLE_VALUE)
	{
		printf(" CreateToolhelp32Snapshot调用失败！ \n");
	}

	BOOL bMore = ::Process32First(hProcessSnap, &pe32);
	while (bMore)
	{
		printf("%ls\n", pe32.szExeFile);

		if (lstrcmp(TEXT("War3.exe"), pe32.szExeFile) == 0|| lstrcmp(TEXT("Frozen Throne.exe"), pe32.szExeFile) == 0)
		{
			printf("QQ 运行中,准备关机\n");
			shutdown();
			break;
		}

		bMore = ::Process32Next(hProcessSnap, &pe32);
	}
	::CloseHandle(hProcessSnap);
	printf("QQ 不在运行\n");
}


int main(int argc, char* argv[])
{
	while (1) {
		checkProcess();
		Sleep(100000);
	}
	system("pause");
	return 0;
}
