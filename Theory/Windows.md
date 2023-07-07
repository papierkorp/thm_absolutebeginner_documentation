#windows #theory
# Versions

- End-User
	- Windows XP
	- Windows Vista
	- Windows 7
	- Windows 8.x
	- Windows 10
		- Home
		- Pro
	- Windows 11
- Server
	- Windows Server 2019

# File System

[File Systems Overview](https://learn.microsoft.com/en-us/troubleshoot/windows-client/backup-and-storage/fat-hpfs-and-ntfs-file-systems)

- FAT16/FAT32 (File Allocation Table)
- HPFS (High Performance File System)
- NTFS (New Technology File System) - Currently used
	- Details
		- NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.
		- Supports files larger than 4GB
		- Set specific permissions on folders and files
		- Folder and file compression
		- Encryption ([Encryption File System](https://docs.microsoft.com/en-us/windows/win32/fileio/file-encryption) or EFS)
		- ADS (Alternate Data Streams)
			- every file has at least one data stream ($DATA) and ADS allows more then one data stream (allows maleware writes to hide data) - [More Information](https://www.malwarebytes.com/blog/news/2015/07/introduction-to-alternate-data-streams)
	- Permissions for files/folders (Rightlick - Properties - Security - Group or user names)

| Permission           | Folders                                                                                                           | Files                                                                                |
|----------------------|-------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| Read                 | Permits viewing and listing of files and subfolders                                                               | Permits viewing or accessing of the files contents                                   |
| Write                | Permits adding of files and subfolders                                                                            | Permits writing to a file                                                            |
| Read & Execute       | Permits viewing and listing of files and subfolders as well as executing of files; inherited by files and folders | Permits viewing and accessing of the files contents as well as executing of the file |
| List Folder Contents | Permits viewing and listing of files and subfolders as well as executing of files; inherited by folders only      | N/A                                                                                  |
| Modify               | Permits reading and writing of files and subfolders; allows deletion of the folder                                | Permits reading and writing of the file; allows deletion of the file                 |
| Full Control         | Permits reading, writing, changing, and deleting of files and subfolders                                          | Permits reading, writing, changing and deleting of the file                          |


# Accounts/Profiles

**See all users**
- Windows Search: `Other users`
- "Win + R" / Rightlick on Start Menu and Run: `lusrmgr.msc`


**Two types of users**
- Administrator
	-  can make changes to the system: add users, delete users, modify groups, modify settings on the system, etc.
- Standard User
	- can only make changes to folders/files attributed to the user & can't perform system-level changes, such as install programs.

**Default Folders for users**
- Desktop
- Documents
- Downloads
- Music
- Pictures

**UAC (User Account Control)**
The default doesnt apply for built-in-local administrator account.
When a user with an account type of administrator logs into a system, the current session doesn't run with elevated permissions. When an operation requiring higher-level privileges needs to execute, the user will be prompted to confirm if they permit the operation to run.

The UAC settings can be changed or even turned off entirely (not recommended).




# Important Folders/Tools

The `%windir%` Environment Variable contains the path to the Windows Operating System.

- `%windir%/system32` - holds important Files, critical for the operating system
- `msconfig` - is for advanced Troubleshooting and helps to diagnose starup issues
	- general = devices and service for Windows to load upon boot
	- boot = boot options
	- services = all configured services
	- startup = empty
	- tools = utilites to configure the os further
- `computermanagement`
	- system tools
		- task scheduler = create and manage common tasks
		- event viewer = view events that have occurred on the computer
			- event types
				- Error = e.g. data loss, loss of functionality
				- Warning = e.g. disk space low
				- Information = e.g. network driver loaded successfully
				- Success Audit = e.g. user successfully logged in
				- Failure Audit = e.g. user access to a network drive failed
			- Default Logs
				- Application = e.g. database application
				- Security = e.g. valid/invalid login attempts
				- System = e.g. failure of driver
				- Custom Log
		- shared folder = see a complete list of shares and folder that others can connect to
			- sessions = list of users connected
			- open files = all currently accessed files
		- local users and groups = local users
		- performance = performance data
		- device manager = view and configure hardware
	- storage
		- windows server backup
		- disk management
			- set up a new drive
			- extend a partition
			- shrink a partition
			- assign or change drive letters
	- services and applications = view properties of service, enable/disable a service
- `msinfo32` (System Information) = displays a comprehensive view of your hardware, system components, and software environment
	- hardware resources
	- components
		- network
			- adapater = shows the ip address
	- software environment
		- environment variables = see all env Variables
			- `Control Panel > System and Security > System > Advanced system settings > Environment Variables`
			- `Settings > System > About > system info > Advanced system settings > Environment Variables`
- `resmon` (Resource Monitor) = displays per-process and aggregate CPU, memory, disk, and network usage information, details about which processes are using individual file handles and modules
- `regedit` ([Windows Registry](https://learn.microsoft.com/en-us/troubleshoot/windows-server/performance/windows-registry-advanced-users)) = central hierarchical database used to store information necessary to configure the system for one or more users, applications, and hardware devices
	- Profiles for each user
	- Applications installed on the computer and the types of documents that each can create
	- Property sheet settings for folders and application icons
	- What hardware exists on the system
	- The ports that are being used.

## Taskmanager

https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/

## Command Prompt (cmd)

useful Commands:

```bash
hostname
whoami #show current user
ipconfig /? #show ip informations
cls #clear the commandline
netstat -a -b -e #protocol statistics and current TCP/IP Network connections
net #manage network resources
net user #see all subcommands..
```


# Active Directory