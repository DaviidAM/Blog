# Powershell Alias y funciones

Es posible modificar el perfil de powershell para agregar alias y funciones personalizadas.

```powershell
notepad $PROFILE
```

Aqui podemos añadir alias y funciones personalizadas que queremos cargar cada vez que abrimos powershell.

```powershell
Set-Alias python G:\Programs\Python3_13\python.exe

function pip {
    & "G:\Programs\Python3_13\python.exe" -m pip @args
}

Set-Alias uv G:\Programs\python_tools\uv\bin\uv.exe

Set-Alias ruff G:\Programs\python_tools\ruff\ruff.exe

Set-Alias oh-my-posh C:\Users\<username>\AppData\Local\Programs\oh-my-posh\bin\oh-my-posh.exe
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH/cloud-native-azure.omp.json" | Invoke-Expression

Import-Module -Name Terminal-Icons
```