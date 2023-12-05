powershell 7以上版本可能已经自带，5.1版本可能需要自己手动安装，管理员身份打开

```markup
#安装
Install-Module -Name PSReadLine -AllowClobber -Force
```

补全，自带的按tab补全命令，也是和linux一样的。

自动补全历史命令：

```markup
#修改启动脚本
notepad.exe $PROFILE

#添加以下内容
# 设置预测文本来源为历史记录，就可以自动补全历史命令，类似于zsh的autosuggestions插件
Set-PSReadLineOption -PredictionSource History
#设置emacs模式，就可以像bash一样，按ctrl a跳转命令行开头，按ctrl e跳转命令行尾
Set-PSReadLineOption -EditMode Emacs

#设置按键，可以根据自己使用习惯调整
##ctrl z 撤销
Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo
#ctrl s 菜单补全，和bash的tab类似
Set-PSReadLineKeyHandler -Key "Ctrl+s" -Function MenuComplete
#按方向键补全历史命令
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward
```

然后重启powershell生效。

其他PSReadLineOption默认选项也可以自己改，还可以改以下输出颜色等等。

```markup
Get-PSReadLineOption

EditMode                               : Windows
AddToHistoryHandler                    : System.Func`2[System.String,System.Object]
HistoryNoDuplicates                    : True
HistorySavePath                        : C:\Users\username\AppData\Roaming\Microsoft\Windows\
                                         PowerShell\PSReadLine\ConsoleHost_history.txt
HistorySaveStyle                       : SaveIncrementally
HistorySearchCaseSensitive             : False
HistorySearchCursorMovesToEnd          : False
MaximumHistoryCount                    : 4096
ContinuationPrompt                     : >>
ExtraPromptLineCount                   : 0
PromptText                             : {> }
BellStyle                              : Audible
DingDuration                           : 50
DingTone                               : 1221
CommandsToValidateScriptBlockArguments : {ForEach-Object, %, Invoke-Command, icm...}
CommandValidationHandler               :
CompletionQueryItems                   : 100
MaximumKillRingCount                   : 10
ShowToolTips                           : True
ViModeIndicator                        : None
WordDelimiters                         : ;:,.[]{}()/\|^&*-=+'"---
AnsiEscapeTimeout                      : 100
CommandColor                           : "`e[93m"
CommentColor                           : "`e[32m"
ContinuationPromptColor                : "`e[97m"
DefaultTokenColor                      : "`e[97m"
EmphasisColor                          : "`e[96m"
ErrorColor                             : "`e[91m"
KeywordColor                           : "`e[92m"
MemberColor                            : "`e[97m"
NumberColor                            : "`e[97m"
OperatorColor                          : "`e[90m"
ParameterColor                         : "`e[90m"
SelectionColor                         : "`e[30;107m"
StringColor                            : "`e[36m"
TypeColor                              : "`e[37m"
VariableColor                          : "`e[92m"
```
