Show ansible commands




All info from servers

```
ansible all -m setup
```

Shell

```
ansible all -m shell -a "ls /var"
ansible all -m shell -a "ls -l /home" -b
```

Copy

```
ansible all -m copy -a 'src=privet.txt dest=/home mode =777"
```

Delete file

```
ansible all -m file -a "path=/home/privet.txt state=absent" -b
```


Download from wesite

```
ansible all -m get_url -a "url= dest=/home" -b
```

Install

apt

```
ansible all -m apt -a "name=stress state=present" -b
ansible heavy -m apt -a "name=apache2 state=present" -b
```

Remove

```
ansible heavy -m apt -a "name=apache2 state=absent" -b
```

Read website from uri

```
ansible all -m uri -a "url=http://concifora.ru return_content=yes"
```

Service

```
ansible heavy -m service -a "name=apache2 state=started enabled=yes" -b
```


Debug

```
ansible heavy -m shell -a "ls /var" -vvv
```