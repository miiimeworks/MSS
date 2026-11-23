========================================================================
             MIIIME Settings Saver (MSS™)
========================================================================

Saves the settings of an installed app into the standard {RawDat} structure of MHL, the portable launcher.
Works in Linked Mode (reads the MHL INI) or Standalone Mode (everything is set in its own INI).
It saves AppData folders (AppDat), settings files in the program folder (Set) and registry keys (Reg).
Can also work as a companion tool for MHL.

설치된 앱의 설정을 포터블 런처 MHL 의 표준 {RawDat} 구조로 저장합니다.
연동 모드(MHL INI 참조)와 독립 모드(자체 INI 에 직접 지정)로 동작합니다.
앱데이터 폴더(AppDat), 프로그램 폴더의 설정 파일(Set), 레지스트리 키(Reg)를 저장합니다.
MHL 의 보조 도구로 사용할 수 있습니다.

========================================================================
[File Organization]
========================================================================

MIIIMESettingsSaver.exe 		# [Run] Saver executable file / [실행] 실행 파일
MIIIMESettingsSaver.ini 			# [Settings] Source, targets and rules / [설정] 소스, 저장 대상 및 규칙
MIIIMESettingsSaver.log 		# [Log] Created when LogLevel is on / [로그] LogLevel 사용 시 생성
MIIIMESettingsSaver_Registry.ini 	# [Output] Created when RegIni=1 / [출력] RegIni=1 일 때 생성되는 MHL [Registry] 초안

========================================================================
[Saved Structure]
========================================================================

{RawDat}\
	AppDat\Local | Roaming | LocalLow\<folder>   # AppData folders / 앱데이터 폴더 (직계 폴더 단위)
  	Reg\<entry name>.reg                         		# Registry keys / 레지스트리 (키 1개 = 파일 1개)
  	Set\<relative path>                          		# Settings files of {Run} / {Run} 의 설정 파일 (하위 폴더 구조 유지)
{RawDat}_bakk\                                 			# Backup of replaced items (one generation) / 교체된 항목의 백업 (1세대)

========================================================================
[How to use]
========================================================================

The three sections [AppDat] / [Set] / [Reg] run in this order.
[Source] / [AppDat] / [Set] / [Reg] each have their own LinkedMode setting that chooses where the source information is read from.

[AppDat] / [Set] / [Reg] 3개 섹션이 이 순서로 실행됩니다.
[Source] / [AppDat] / [Set] / [Reg] 는 각각 LinkedMode 설정을 가지며, 소스 정보를 어디에서 읽을지 선택합니다.

--- Standalone Mode / 독립 모드 ---

1. Open [MIIIMESettingsSaver.ini], set LinkedMode=0 and fill in [Source]
   (AppName, RunPath, and Output if you want a different save path).
2. Add the entries you need to [AppDat] / [Set] / [Reg].
3. Run [MIIIMESettingsSaver.exe] with Preview=1 first and check the log, then set Preview=0 and run again.

1. [MIIIMESettingsSaver.ini]를 열어 LinkedMode 를 0 으로 두고 [Source] 를 채움
   (AppName, RunPath, 저장 경로를 따로 두려면 Output).
2. [AppDat] / [Set] / [Reg] 에 필요한 항목을 추가.
3. 먼저 Preview=1 로 [MIIIMESettingsSaver.exe]를 실행하여 로그를 확인한 뒤, Preview=0 으로 바꿔 다시 실행.

--- Linked Mode / 연동 모드 ---

LinkedMode=1 reads the source information of that section from the MHL INI found by LinkedIniName
in the same path as MIIIMESettingsSaver.exe.
  [Source] = RawDataPath (save path) / [AppDat] = AppName / [Set] = RunPath / [Reg] = AppName + the [Registry] entries of the MHL INI.
Extra entries (vendor folders, extensions, exclude lists, extra registry keys) are always written in MIIIMESettingsSaver.ini
and are added on top of the source information.
The MHL INI is searched only when at least one section has LinkedMode=1.
A section without LinkedMode is treated as 0.

LinkedMode=1 이면 그 섹션의 소스 정보를 MIIIMESettingsSaver.exe 와 동일한 경로에서 LinkedIniName 으로 찾은
MHL INI 에서 읽습니다.
  [Source] = RawDataPath(저장 경로) / [AppDat] = AppName / [Set] = RunPath / [Reg] = AppName + MHL INI 의 [Registry] 항목.
추가 항목(벤더 폴더, 확장자, 제외 목록, 추가 레지스트리 키)은 연동 모드에서도 MIIIMESettingsSaver.ini 에 적으며,
소스 정보에 더해서 적용됩니다.
MHL INI 는 LinkedMode=1 인 섹션이 하나라도 있을 때만 찾습니다.
LinkedMode 가 없는 섹션은 0 으로 처리됩니다.

LinkedIniName ([Options]) is a priority list separated by pipes (|). e.g. |*_x64_M|*_M|*_x86_M|
Entries are searched in order, and the first entry that finds an .ini file is used.
An error occurs if one entry finds two or more files, or if no entry finds a file.

LinkedIniName([Options])은 파이프(|)로 구분된 우선순위 목록입니다. 예: |*_x64_M|*_M|*_x86_M|
앞에서부터 차례로 탐색하여 .ini 파일이 발견된 첫 항목을 사용합니다.
한 항목에서 2개 이상 발견되거나 어느 항목에서도 발견되지 않으면 에러가 발생합니다.

※ Setting tips / 설정 팁 :
 - [Source] : Source information for standalone mode. AppName, RunPath (install folder searched by [Set]), Output (= {RawDat}; if empty, MSSfolder\{AppName}\App\RawDat is used).
   / 독립 모드용 소스 정보. AppName, RunPath(설치 폴더, [Set] 의 검색 대상), Output(= {RawDat}. 비우면 MSS폴더\{AppName}\App\RawDat 사용).
 - [AppDat] : Saves folders of Local / Roaming / LocalLow as they are (direct subfolders only). The folder named like AppName is found automatically (AutoAppName=1). Add vendor folders as 01=Roaming|VendorName (wildcards ~ ^ need 3+ normal characters).
   / Local / Roaming / LocalLow 의 폴더를 그대로 저장 (직계 폴더만). AppName 과 같은 이름의 폴더는 자동으로 찾습니다(AutoAppName=1). 벤더 폴더는 01=Roaming|VendorName 형식으로 추가 (와일드카드 ~ ^ 는 일반 글자 3자 이상 필요).
 - [Set] : Saves settings files of the program folder (Ext / Include / Exclude / Depth / MaxSizeKB) keeping the subfolder structure. Files identical to the saved copy are not saved again.
   / 프로그램 폴더의 설정 파일(Ext / Include / Exclude / Depth / MaxSizeKB)을 하위 폴더 구조를 유지하여 저장. 저장본과 같은 파일은 다시 저장하지 않습니다.
 - [Reg] : Saves each registry key as Reg\<entry name>.reg (same entry names as [Registry] of the MHL INI, e.g. 01 -> 01.reg). AutoAppName=1 adds HKCU\SOFTWARE\{AppName} as 00.reg. Value-level entries (Key|ValueName) and shared areas (FileExts / OpenWithProgids) are not supported.
   / 레지스트리 키를 Reg\<항목 이름>.reg 로 저장 (MHL INI 의 [Registry] 와 같은 항목 이름. 예: 01 -> 01.reg). AutoAppName=1 이면 HKCU\SOFTWARE\{AppName} 를 00.reg 로 저장. 값 단위 항목(Key|ValueName)과 공용 영역(FileExts / OpenWithProgids)은 지원하지 않습니다.
 - RegIni=1 : Writes the saved keys as MIIIMESettingsSaver_Registry.ini in the MHL [Registry] / [RegistryRoot] format, ready to copy into the MHL INI.
   / 저장한 키를 MHL 의 [Registry] / [RegistryRoot] 형식으로 MIIIMESettingsSaver_Registry.ini 에 저장. MHL INI 에 복사해 쓸 수 있는 초안입니다.
 - Preview=1 : Nothing is created in {RawDat}; the list of what would be saved goes to the log (LogLevel is raised to 1 automatically). The registry is only read.
   / {RawDat} 에 아무것도 만들지 않고 저장될 목록만 로그에 기록합니다 (LogLevel 이 자동으로 1 로 올라갑니다). 레지스트리는 읽기만 합니다.
 - Overwrite=1 : Existing items are moved to {RawDat}_bakk (one generation) before being saved. Overwrite=0 skips them.
   / 이미 있는 항목은 {RawDat}_bakk (1세대)로 옮긴 뒤 저장. Overwrite=0 이면 건너뜁니다.
 - Registry view : HKLM\SOFTWARE is read in the registry view of the MSS build (x86 build = 32-bit apps, x64 build = 64-bit apps).
   / HKLM\SOFTWARE 는 MSS 빌드의 레지스트리 뷰에서 읽습니다 (x86 빌드 = 32비트 앱, x64 빌드 = 64비트 앱).
 - Close the target app before saving. Files in use cannot be copied.
   / 저장하기 전에 대상 앱을 종료하십시오. 사용 중인 파일은 복사할 수 없습니다.
 - Safety / 안전장치 : Drive roots, system folders (Windows, Program Files, etc.) as the save path, Junction / symbolic-link folders and user data folders (Documents, Desktop, etc.) are blocked by default. Registry keys are limited to HKCU\SOFTWARE / HKLM\SOFTWARE; Run, Policies and similar keys are blocked.
   / 저장 경로가 드라이브 루트 / 시스템 폴더(Windows, Program Files 등)이거나, Junction / 심볼릭 링크 폴더, 사용자 데이터 폴더(문서, 바탕화면 등)는 기본적으로 차단됩니다. 레지스트리는 HKCU\SOFTWARE / HKLM\SOFTWARE 만 허용하며 Run, Policies 등은 차단됩니다.

______________________________________________________________________________________________________________________

This program was created with AutoIt. Some antivirus programs may incorrectly detect it as a virus.
본 프로그램은 AutoIt으로 제작되었습니다. 일부 백신이 바이러스로 오진 할 수 있습니다.

========================================================================
[Disclaimer]
========================================================================

Provided **"AS IS"**, without warranty.
This is a **private project**. No technical support is provided.
본 프로그램은 **"있는 그대로"** 제공되며, 사용 중 발생하는 문제에 대해 제작자는 책임을 지지 않습니다.
기술 지원은 제공되지 않습니다.

Developer	: MIIIME 
Website		: https://www.miiime.com 
GitHub		: @miiimeworks 
Update      	: 2026.11.23
