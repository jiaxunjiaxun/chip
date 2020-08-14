http://xstarcd.github.io/wiki/windows/windows_cmd_syntax.html

https://ss64.com/


## 批量修改文件创建时间

``` shell
# 修改为当前日期
ls 'path\to\file\*.*' | foreach-object { $_.LastWriteTime = Get-Date; $_.CreationTime = Get-Date }

# 修改为指定日期
ls 'path\to\file\*.*' | foreach-object { $_.LastWriteTime = '01/11/2004 22:13:36'; $_.CreationTime = '01/11/2004 22:13:36' }

# 递归处理
Get-Childitem -path 'path\to\file\' -Recurse | foreach-object { $_.LastWriteTime = Get-Date; $_.CreationTime = Get-Date }
```

