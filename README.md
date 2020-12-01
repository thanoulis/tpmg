**tpmg** is a 'Poor Mans's GUI' much like [zenity](https://gitlab.gnome.org/GNOME/zenity) or [kdialog](https://invent.kde.org/utilities/kdialog) written in core [Tcl/Tk](https://www.tcl.tk).

### Dialogs
* [Color](#color-dialog "Color")
* [Directory](#directory-dialog "Directory")
* [FileSelect](#fileselect-dialog "FileSelect")
* [FileSave](#filesave-dialog "FileSave")
* [Information](#information-dialog "Information")
* [Password](#password-dialog "Password")
* [Entry](#entry-dialog "Entry")
* [Scale](#scale-dialog "Scale")
* [List](#list-dialog "List")
* [Progress](#progress-dialog "Progress")

#### Color dialog
* description:<br/>
  Displays a color selection dialog. Appearance depends on platform.
* options:
  | option   | type     | description       | default           |
  |----------|:--------:|-------------------|:-----------------:|
  | --title= | string   | set window title  | "Select a Color"  |
  | --color= | hexcolor | set initial color | "#d9d9d9"         |
  | --help   |          | this help         |
* returns:
  | OK    | Cancel | Error |
  |:-----:|:------:|:-----:|
  | color | 1      | 255   |
* example:
  ```shell
  tpmg --color --title="Choose Color" --color="#eed421"
  ```
* screenshot:<br/>
  ![ColorDialog](screenshots/ColorDialog.png "ColorDialog")

#### Directory dialog
* description:<br/>
  Display a directory select dialog. Appearance depends on platform.
* options:
  | option   | type   | description          | default              |
  |----------|:------:|----------------------|:--------------------:|
  | --title= | string | set window title     | "Select a Directory" |
  | --exist  |        | directory must exist |
  | --help   |        | this help            |
* returns:
  | OK        | Cancel | Error |
  |:---------:|:------:|:-----:|
  | directory | 1      | 255   |
* example:
  ```shell
  tpmg --directory --title="Select a Directory" --exist
  ```
* screenshot:<br/>
  ![DirectoryDialog](screenshots/DirectoryDialog.png "DirectoryDialog")

#### FileSelect dialog
* description:<br/>
  Display a file selection dialog. Appearance depends on platform.
* options:
  | option    | type   | description           | default        |
  |-----------|:------:|-----------------------|:--------------:|
  | --title=  | string | set window title      | "Select Files" |
  | --ext=    | csv    | set filetype filter   | show all files |
  | --single  |        | single file selection |
  | --help    |        | this help             |
* returns:
  | OK        | Cancel | Error |
  |:---------:|:------:|:-----:|
  | file list | 1      | 255   |
* example:
  ```shell
  tpmg --fileselect --title="Select a File" --ext="*.txt,*" --single
  ```
* screenshot:<br/>
  ![FileSelectDialog](screenshots/FileSelectDialog.png "FileSelectDialog")

#### FileSave dialog
* description:<br/>
  Display a file save dialog. Appearance depends on platform.
* options:
  | option       | type   | description                 | default           |
  |--------------|:------:|-----------------------------|:-----------------:|
  | --title=     | string | set window title            | "Save File"       |
  | --ext=       | csv    | set filetype filter         | show all files    |
  | --file=      | path   | set initial file            |
  | --noconfirm  |        | do not confirm on overwrite | confirm overwrite |
  | --help       |        | this help                   |
* returns:
  | OK        | Cancel | Error |
  |:---------:|:------:|:-----:|
  | file path | 1      | 255   |
* example:
  ```shell
  tpmg --filesave --title="Save File" --file="~/myfile.txt" --noconfirm
  ```
* screenshot:<br/>
  ![FileSaveDialog](screenshots/FileSaveDialog.png "FileSaveDialog")

#### Information dialog
* description:<br/>
  Display an information dialog. Supports icons for info, error, question and warning.<br/>
  Can have a combination of buttons, as okcancel, yesno, retrycancel, etc.<br/>
  Main message is bold and equals to the first non option string.<br/>
  All other strings are message details, with every string represent a line.
* options:
  | option       | type               | description                 | default |
  |--------------|:------------------:|-----------------------------|:-------:|
  | --title=     | string             | set window title            |
  | --icon=      | icon<sup>1</sup>   | icon to use in dialog       | info    |
  | --button=    | button<sup>2</sup> | buttons to use in dialog    | ok      |
  | first string | string             | main message (in bold)      |
  | other string | string             | message details             |  
  | --help       |                    | this help                   |
>1: info error question warning<br/>
>2: ok okcancel yesno yesnocancel retrycancel abortretryignore
* returns:<br/>
  the button<sup>2</sup> name
* example:
  ```shell
  tpmg --information --title="Are you sure?" --icon="question" \
    --button="yesnocancel" "All data will be wiped!" \
    "This action cannot be undone." "Proceed?"
  ```
* screenshot:<br/>
  ![InformationDialog](screenshots/InformationDialog.png "InformationDialog")

#### Password dialog
* description:<br/>
  Display a classic username/password dialog.<br/>
  Username entry can be omitted.<br/>
  Password entry hides text with asterisks.
* options:
  | option        | type   | description                 | default      |
  |---------------|:------:|-----------------------------|:------------:|
  | --title=      | string | set window title            | "Login As"   |
  | --nousername  |        | hide the "Username" entry   |
  | --help        |        | this help                   |
* returns:
  | OK                    | Cancel | Error |
  |:---------------------:|:------:|:-----:|
  | username<br/>password | 1      | 255   |
* example:<br/>
  ```shell
  tpmg --password --title="Welcome $USER" --nousername
  ```
* screenshot:<br/>
  ![PasswordDialog](screenshots/PasswordDialog.png "PasswordDialog")

#### Entry dialog
* description:<br/>
  Display any number of entry dialogs. Default is one.<br/>
  Configuration is done through `--text` option, with comma separated label texts, as in example below.
* options:
  | option        | type   | description      | default             |
  |---------------|:------:|------------------|:-------------------:|
  | --title=      | string | set window title | "Enter Text"        |
  | --text=       | csv    | set label text   | "Enter text below:" |
  | --help        |        | this help        |
* returns:
  | OK         | Cancel | Error |
  |:----------:|:------:|:-----:|
  | entry list | 1      | 255   |
* example:
  ```shell
  tpmg --entry --title="Personal Information" \
    --text="First name:,Last name:,email:"
  ```
* screenshot:<br/>
  ![EntryDialog](screenshots/EntryDialog.png "EntryDialog")

#### Text dialog
* description:<br/>
  Display a text information dialog.<br/>
  Text can be from file, from command line in the form of every string is a new line and from standard input.<br/>
  In case of multiple inputs, will concatenate the text, with `file`->`string`->`stdin` hierarchy.
* options:
  | option        | type   | description       | default       |
  |---------------|:------:|-------------------|:-------------:|
  | --title=      | string | set window title  | "Show Text"   |
  | --file=       | path   | text file to show |
  | --edit        |        | can edit text     | cannot edit   |
  | --font=       | font   | font to use       | "TkFixedFont" |
  | other strings | string | text body (every string is a new line) |  
  | --help        |        | this help         |
* returns:  
  | OK | Cancel | Error |
  |:--:|:------:|:-----:|
  | ok | 1      | 255   |
* example:
  ```shell
  tpmg --text --title="README" --file="~/README.txt" \
    --edit --font="{DejaVu Sans Mono} 12 bold"
  ```
* screenshot:<br/>
  ![TextDialog](screenshots/TextDialog.png "TextDialog")

#### Scale dialog
* description:<br/>
  Display a scale dialog. Min, max and current value can be configured.
* options:
  | option   | type    | description       | default               |
  |----------|:-------:|-------------------|:---------------------:|
  | --title= | string  | set window title  | "Adjust Value"        |
  | --text=  | string  | set label text    | "Adjust value below:" |
  | --min=   | integer | set min value     | 0                     |
  | --max=   | integer | set max value     | 100                   |
  | --value= | integer | set initial value | 0                     |
  | --help   |         | this help         |
* returns:
  | OK    | Cancel | Error |
  |:-----:|:------:|:-----:|
  | scale | 1      | 255   |
* example:
  ```shell
  tpmg --scale --title="Adjust Transparency" \
    --text="Choose window transparency:" \
    --min="0" --max="100" --value="20"
  ```
* screenshot:<br/>
  ![ScaleDialog](screenshots/ScaleDialog.png "ScaleDialog")

#### List dialog
* description:<br/>
  Display an option list with radio or check buttons. Default is check buttons.<br/>
  Configuration is done through `--options` option, with comma separated list, much like in **Entry** dialog.<br/>
  Can also set the "default" option in a radio list, or toggle default values in the check list.<br/>
  The following example shows how.
* options:
  | option     | type         | description       | default              |
  |------------|:------------:|-------------------|:--------------------:|
  | --title=   | string       | set window title  | "Set Options"        |
  | --text=    | string       | set label text    | "Set options below:" |
  | --type=    | check\|radio | set list type     | check                |
  | --options= | csv          | set options list  |
  | --default= | radio:string<br/>check:csv | radio:default option<br/>check:set option to true |
  | --anchor=  | w\|e\|c      | list placement in window | w             |
  | --help     |              | this help         |
* returns:
  | OK                      | Cancel | Error |
  |:-----------------------:|:------:|:-----:|
  | radio: selected option  | 1      | 255   |
  | check: true\|false list | 1      | 255   |
* example:
  ```shell
  tpmg --list --title="Select filetype" --text="Select filetype:" \
    --type="radio" --options="Text File,RTF Document,Word Document" \
    --default="Text File" --anchor="w"
  ```
* screenshot:<br/>
  ![ListDialog](screenshots/ListDialog.png "ListDialog")

#### Progress dialog
* description:<br/>
  Display a progress bar dialog.<br/>
  **Progress** dialog reads data from `stdin` line by line.<br/>
  Lines must be prefixed with `tpmg:`, or are ignored.<br/>
  If text is a number, the progress bar advances to that number.<br/>
  Else, it updates the label text.
* options:
  | option   | type     | description                | default         |
  |----------|:--------:|----------------------------|:---------------:|
  | --title= | string   | set window title           | "Show Progress" |
  | --text=  | string   | set label text             |
  | --color= | hexcolor | set progress bar color     | Tk default      |
  | --pulse  |          | pulsating progress bar     |
  | --auto   |          | close window on completion |
  | --max=   | integer  | set max value              | 100             |
  | --value= | integer  | set initial bar value      | 0               |
  | --help   |          | this help                  |
* returns:
  | OK | Cancel | Error |
  |:--:|:------:|:-----:|
  | ok | 1      | 255   |
* example:
  ```shell
  #!/usr/bin/env sh
  (
  echo "tpmg:Starting jobs..."; sleep 1
  echo "tpmg:30"; echo "tpmg:Setting variables..."; sleep 1
  echo "tpmg:70"; echo "tpmg:Clearing cache..."; sleep 1
  echo "This line will be ignored"; sleep 1
  echo "tpmg:100"; echo "tpmg:Done."
  ) | tpmg --progress --color="#948b84" --auto
  ```
* screenshot:<br/>
  ![ProgressDialog](screenshots/ProgressDialog.png "ProgressDialog")
* bugs:<br/>
  Wrong behavior on pulsating progress bar (not critical).


### Dependencies
* **Tcl** version 8.6 or later.
* **Tk** version 8.6 or later.
##### For Microsoft Windows users:
* [ActiveTcl](https://www.activestate.com/activetcl) version 8.6 or later.


### License
**tpmg** is licensed under the **MIT License**.

Read [LICENSE](LICENSE) for details.
