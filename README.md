# azure

import commands as sp
from azure.storage.file import FileService
from azure.storage.file import ContentSettings
from datetime import datetime


now = datetime.now()
date=now.strftime("%d")
backup_dir="backup_on_"+date

# put the values of myaccount and mykey as ur accnt
file_service = FileService(account_name='myaccount', account_key='mykey')
file_service.create_share('mybackup')
file_service.create_directory('mybackup', '{}'.format(backup_dir))

# for NAMES OF ALL FILES TO BACKEDUP IN /ROOT/DB FOLDER
name_path="/root/index/names_"+backup_dir
db_path="/root/db/"

files=sp.getoutput("ls "+db_path)
names = files.split("\n")

sp.getoutput(" echo /root/db/"+name[0]+" > "+name_path)
for i in names[0:]:
        sp.getoutput("echo /root/db/"+i+" >> "+name_path)



for x in names:
        file_service.create_file_from_path(
        'mybackup',
        '{0}',
        '{1}',
        '{1}',content_settings=ContentSettings(content_type='text/txt').format(backup_dir,x))

