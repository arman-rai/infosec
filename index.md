So one of my findings is that the system binaries can only be found once the python venv is loaded
so this is valid
`──(volaty3)─(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ which vol    
/home/namura/venvs/volaty3/bin/vol
                                                                              
┌──(volaty3)─(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ deactivate  
                                                                              
┌──(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ which vol
vol not found
                                                                              
┌──(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ source volaty3/bin/activate        
source: no such file or directory: volaty3/bin/activate
                                                                              
┌──(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ source ~/venvs/volaty3/bin/activate
                                                                              
┌──(volaty3)─(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ ls
activate      activate.fish  pip   pip3.13  python3     vol
activate.csh  Activate.ps1   pip3  python   python3.13  volshell
                                                                              
┌──(volaty3)─(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ which vol
/home/namura/venvs/volaty3/bin/vol
`
Some forensics are: 
┌──(volaty3)─(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ python3 vol -f /home/namura/test/Challenge.raw windows.info
gives OS build, acquisition timestamp, memory layout details

┌──(volaty3)─(namura㉿namu)-[~/venvs/volaty3/bin]
└─$ python3 vol -f /home/namura/test/Challenge.raw windows.pslist
View running and recently terminated processes, filter for anomalies.

