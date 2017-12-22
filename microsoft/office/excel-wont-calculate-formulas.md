<!-- TITLE: Excel Wont Calculate Formulas -->

Delete the « /e » into the following registry keys: 


```text
HKEY_CLASSES_ROOT\Excel.Sheet.8\shell\Open\command 
HKEY_CLASSES_ROOT\Excel.Template\shell\Open\command 

"(Standard)"="C:\Programs\Microsoft Office\OFFICE11\EXCEL.EXE" /e 
"command"=']gAVn-}f(ZXfeAR6.jiEXCELFiles>!De@]Vz(r=f`1lfq`?R& /e
```
