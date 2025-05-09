## ConfigDatabase:
  - `Public Sub InitConfig()`
    - Initializes the Winblows configuration database. Should only be used by the boot process.
  - `Public Sub DeInitConfig()`
    - De-initializes the Winblows configuration database. Should only be used by the shutdown process.

## AppMarker:
  (note: AppMarker is the interface that's required to implement for creating Winblows applications.)
  - `Function GetApplication() As WinbForm`
    - Should return an instance of the WinbForm of the main window of your application. If your application uses CustomApp, this should return Nothing.
  - `Function GetCustomApplication() As CustomApp`
    - Should return an instance of a CustomApp structure. Used if your application does not use a WinbForm.

## CustomApp:
  - `Public Sub New(frm As Form, Optional pname As String = Nothing)`
    - Creates a new instance of the CustomApp structure.

## btoskrnl:
  - `Public Sub AsyncSleep(ms As Int32, callback As [Delegate])`
    - Sleeps for the specified amount of milliseconds on a separate thread and then calls the specified callback delegate.
  - `Public Function GetInternalImageResource(name As String) As Bitmap`
    - Returns a Bitmap of the image resource with the specified name. If the resource does not exist, it returns a placeholder.
  - `Public Function Rnd(min, max) As Integer`
    - Generates a random number in the specified range.
  - `Public Sub LaunchAppFromFile(realpath As String)`
    - Launches the .EZE application specified. (note: this uses real paths. use DecodePath if this needs to be used with Winblows paths)
  - `Public Sub LaunchAppFromFile(realpath As String, args As String)`
    - Launches the .EZE application specified with the specified arguments. (note: this uses real paths. use DecodePath if this needs to be used with Winblows paths)
  - `Public Sub RawLaunchForm(frm As Form)`
    - Directly shows the specified Form inside the Winblows environment. Do not use this, use LaunchFormEx instead.
  - `Public Function LaunchFormEx(frm As WinbForm, Optional args As String = "") As WinbForm`
    - Launches the specified WinbForm into the Winblows environment with optionally the specified arguments.
  - `Public Function LaunchFormEx(app As ConsoleApplication, Optional args As String = "") As MemoryStream`
    - Unimplemented.
  - `Public Sub LaunchForm(frm As Form, Optional args As String = "")`
    - Launches the specified Form into the Winblows environment. Should be used if the specified form is not a WinbForm.
      - (note: if a WinbForm is specified, it will still show. this is for backwards compatibility and should not be used in new applications.)

## UserProperties:
  - `Public Sub New(phash As Int32, dir As String)`
    - Creates a new instance of the UserProperties class.

## Debug:
  - `Public Sub InitDebug()`
    - Initializes the Winblows debug server. Should only be used in the boot process. (note: in release builds the debug server does not work.)
  - `Public Sub PrintLn(str As String)`
    - Prints the specified text to the debugger followed by a new-line character. (note: in release builds printing text to the debugger does not work.)
  - `Public Sub Print(str As String)`
    - Prints the specified text to the debugger, excluding a new-line character. (note: in release builds printing text to the debugger does not work.)

## FramebufConsole:
  (note: this class is deprecated and should not be used. it will still be documented for archival purposes)
  - `Public Sub WriteLine(str As String)`
    - Writes the specified string followed by a new-line character to the Framebuffer console.
  - `Public Sub Write(str As String)`
    - Writes the specified string to the Framebuffer console.
  - `Public Sub SetStatusBar(str As String)`
    - Sets the text on the status bar of the Framebuffer console.
  - `Public Sub Clear()`
    - Clears the text on the Framebuffer console.
  - `Public Sub EnableConsole()`
    - Enables the Framebuffer console.
  - `Public Sub DisableConsole()`
    - Disables the Framebuffer console.

## LoginSubsystem:
  - `Public Sub WinbUserLogOff()`
    - Logs off the currently logged in user. Halfplemented, should not be used.
  - `Public Function GetUserProperties(username As String) As UserProperties`
    - Returns the UserProperties object associated with the specified user.
  - `Public Sub SetUserProperties(username As String, data As UserProperties)`
    - Sets the user data of the specified user to the provided UserProperties object.
  - `Public Sub WinbCreateUser(username As String, passwd As String)`
    - Creates a Winblows user with the specified user name and password. The UserProperties will be set to defaults.
  - `Public Function WinbUserLogIn(username As String, passwdHash As Integer) As Int32`
    - Tries to log in to the specified user, provided the hashed password is correct.
    - This returns 0 if the log-in is successful. 1 is returned if the provided credentials are incorrect. -1 is returned if there are no registered users on the system, this should never happen.
  - `Public Sub InitUPS()`
    - Initializes the Winblows login system. Should only be used by the boot process.
  - `Public Sub TryCreateUserFolders()`
    - Creates the user directories (documents, pictures etc.) inside the user folder of the currently logged in user.
    - If one of the directories already exists, it skips that directory.
  - `Public Function GetUserFolder(foldername As UserFolders)`
    - Returns the path to the specified UserFolder of the currently logged-in user.

## Processes:
  - `Public Function GetProcesses() As List(Of Process)`
    - Returns a list of the currently running processes.
  - `Public Function GetApps() As List(Of Process)`
    - Returns a list of the currently running applications.
  - `Public Function CreateCustomProcess(proc As CustomApp)`
    - Creates a process from the specified CustomApp and runs the process.
      - (note: processes created this way can never properly exit due to the way that CustomApp works)
  - `Public Function CreateProcess(proc As Process, Optional args As String = "") As Object`
    - Runs the specified process, optionally with the arguments specified.
  - `Public Function KillProcess(proc As Process) As Integer`
    - Kills the specified Process.
  - `Public Function KillProcess(PID As Integer) As Integer`
    - Kills the process with the specified PID.
  - `Public Function KillProcess(ProcessName As String) As Integer`
    - Kills all processes with the specified process name.

## Process:
  - `Public Sub Dispose()`
    - Exits and disposes the current process.
  - `Public Sub New(ProcName As String, form As WinbForm, args As String, Optional assem As Assembly = Nothing)`
    - Creates a new instance of the Process class with the specified process name, WinbForm, arguments and optionally the assembly. 
      - (note: the assem argument should only be used when launching a process from a .EZE file)
  - `Public Sub New(ProcName As String)`
    - Creates a new "fake" process with the specified process name. Used for testing purposes.
      - (note: processes created this way cannot be killed)
  - `Public Function Run() As Object`
    - Runs the specified process. Should only be ran by the Processes module, not on its own.
  - `Public Sub ExitProcess()`
    - Exits the specified process. Instead of using ExitProcess, properly dispose of the Process.

## RealMode:
  - `Public Sub WinbSwitchRealMode()`
    - Switches to the Winblows Real Mode. Should only be used by the boot process.
  - `Public Sub WinbSwitchProcMode()`
    - Switches to the Winblows Protected Mode. Should only be called by the shutdown process.
  - `Public Sub WinbSwitchVidMode(width As Int32, height As Int32)`
    - Switches the current video mode to the specified parameters.
  - `Public Sub WinbShutDownOperatingSystem()`
    - Shuts down the Winblows operating system.
  - `Public Sub WinbRestartOperatingSystem()`
    - Restarts the Winblows operating system.
  - `Public Sub WinbSwitchToUserMode()`
    - Switches to User (Winb34) mode.
  - `Public Sub WinbSwitchToFramebufferMode()`
    - Switches to Framebuffer (Native) mode.

## WinbVersion:
  - `Public Function GetVersionString() As String`
    - Gets the version string and copyright string of the current Winblows build.

## Shell34.ShortcutTools:
  - `Public Sub CreateShortcut(sc As Shortcut, winbpath As String)`
    - Creates a new shortcut at the specified Winblows path.
  - `Public Function GetShortcut(scpath As String) As Shortcut`
    - Gets the Shortcut object at the specified Winblows path.
  - `Public Sub RunShortcut(scpath As String)`
    - Runs the program associated with the shortcut at the specified Winblows path.
  - `Public Sub RunShortcut(scpath As String, args As String)`
    - Runs the program associated with the shortcut at the specified Winblows path with the provided arguments.
  - `Public Sub RunShortcut(sc As Shortcut)`
    - Runs the program associated with the specified Shortcut.
  - `Public Sub RunShortcut(sc As Shortcut, args As String)`
    - Runs the program associated with the specified Shortcut with the provided arguments.

## Shell34.OldShell34:
  - `Public Function ShowMessageBox(text As String, Optional title As String = "Maikroshet Winblows", Optional cancelButton As Boolean = False)`
    - Shows a Winblows message box with the specified text and optionally a different title and optionally with the cancel button enabled.
  - `Public Function ShowMessageBox(text As String, img As Image, Optional title As String = "Maikroshet Winblows", Optional cancelButton As Boolean = False)`
    - Shows a Winblows message box with the specified text and icon and optionally a different title and optionally with the cancel button enabled.
  - `Public Function ReadAllText(filepath As String) As String`
    - Reads all text from the specified path. Is compatible with both Winblows and real paths.
  - `Public Function DecodePath(path As String) As String`
    - Converts from a Winblows path to a real path.
  - `Public Function EncodePath(path As String) As String`
    - Converts from a real path to a Winblows path.
  - `Public Function CheckIfPathValid(path As String) As Boolean`
    - Checks if the specified Winblows path is valid.
  - `Public Function GenRndNum(min As Int32, max As Int32)`
    - Generates a random number within the specified range.
  - `Public Sub SetBackImageToSolidColor(color As Color)`
    - Sets the background to the specified solid color. Does not persist across reboots.
  - `Public Sub SetBackImage(img As Image)`
    - Sets the background to the specified image. Does not persist across reboots.
  - `Public Sub RefreshWallpaper()`
    - Refreshes the current background.
  - `Public Sub LaunchFileViaShell(filename As String)`
    - Launches the file at the specified Winblows path. 
    - If the file has an unknown extension, this will give a popup asking the user how to open the file.s