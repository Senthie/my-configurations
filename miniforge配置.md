# minifoget 在poswershell 上的 配置

```ps1

# 禁用自动提示符
$env:CONDA_CHANGEPS1 = 0

# 仅初始化conda（如果要用mamba命令，可以跳过mamba初始化，直接用conda）
If (Test-Path "C:\ProgramData\miniforge3\Scripts\conda.exe") {
    (& "C:\ProgramData\miniforge3\Scripts\conda.exe" "shell.powershell" "hook") | Out-String | Where-Object {$_} | Invoke-Expression
}
#region mamba initialize
# !! Contents within this block are managed by 'mamba shell init' !!
$Env:MAMBA_ROOT_PREFIX = "C:\ProgramData\miniforge3"
$Env:MAMBA_EXE = "C:\ProgramData\miniforge3\Library\bin\mamba.exe"
(& $Env:MAMBA_EXE 'shell' 'hook' -s 'powershell' -r $Env:MAMBA_ROOT_PREFIX) | Out-String | Invoke-Expression
#endregion

# 最简单的提示符
function prompt {
    if ($env:CONDA_DEFAULT_ENV) {
        Write-Host -NoNewline "($env:CONDA_DEFAULT_ENV) " -ForegroundColor Green
    }
    Write-Host -NoNewline "PS $PWD>" -ForegroundColor Blue
    return " "
}


```
