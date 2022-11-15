"# Lesson3.1_HDFS" 
PS C:\Scripts\docker-hadoop-spark-workbench-master> docker cp c:\Users\kolyaginkk\Downloads\voyna-i-mir-tom-1.txt 6ab489fbd96a129d1b55ea9021bcc6c803507641c215debb95117d10964e0c1c:/user/cloudera
PS C:\Scripts\docker-hadoop-spark-workbench-master> docker cp c:\Users\kolyaginkk\Downloads\voyna-i-mir-tom-2.txt 6ab489fbd96a129d1b55ea9021bcc6c803507641c215debb95117d10964e0c1c:/user/cloudera
PS C:\Scripts\docker-hadoop-spark-workbench-master> docker cp c:\Users\kolyaginkk\Downloads\voyna-i-mir-tom-3.txt 6ab489fbd96a129d1b55ea9021bcc6c803507641c215debb95117d10964e0c1c:/user/cloudera
PS C:\Scripts\docker-hadoop-spark-workbench-master> docker cp c:\Users\kolyaginkk\Downloads\voyna-i-mir-tom-4.txt 6ab489fbd96a129d1b55ea9021bcc6c803507641c215debb95117d10964e0c1c:/user/cloudera

PS C:\Scripts\docker-hadoop-spark-workbench-master> docker exec -it 6ab489fbd96a bash
root@6ab489fbd96a:/# cd /user/cloudera/
root@6ab489fbd96a:/user/cloudera# ls
voyna-i-mir-tom-1.txt  voyna-i-mir-tom-2.txt  voyna-i-mir-tom-3.txt  voyna-i-mir-tom-4.txt  voyna-i-mir.txt 

root@6ab489fbd96a:/user/cloudera# hdfs dfs -copyFromLocal voyna-i-mir-tom-1.txt voyna-i-mir-tom-2.txt voyna-i-mir-tom-3.txt voyna-i-mir-tom-4.txt /user/cloudera

-- После того, как файлы окажутся на HDFS попробуйте выполнить команду, которая выводит содержимое папки. Особенно обратите внимание на права доступа к вашим файлам.
root@6ab489fbd96a:/user/cloudera# hdfs dfs -ls /user/cloudera
Found 4 items
-rw-r--r--   3 root cloudera     736519 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-1.txt
-rw-r--r--   3 root cloudera     770324 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-2.txt
-rw-r--r--   3 root cloudera     843205 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-3.txt
-rw-r--r--   3 root cloudera     697960 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-4.txt

-- Далее сожмите все 4 тома в 1 файл.
root@6ab489fbd96a:/user/cloudera# hdfs dfs -getmerge /user/cloudera/voyna-i-mir-tom-1.txt /user/cloudera/voyna-i-mir-tom-2.txt /user/cloudera/voyna-i-mir-tom-3.txt /user/cloudera/voyna-i-mir-tom-4.txt /user/cloudera/voyna-i-mir.txt
root@6ab489fbd96a:/user/cloudera# ls
voyna-i-mir-tom-1.txt  voyna-i-mir-tom-2.txt  voyna-i-mir-tom-3.txt  voyna-i-mir-tom-4.txt  voyna-i-mir.txt

root@6ab489fbd96a:/user/cloudera# hdfs dfs -copyFromLocal voyna-i-mir.txt /user/cloudera
root@6ab489fbd96a:/user/cloudera# hdfs dfs -ls /user/cloudera
Found 5 items
-rw-r--r--   3 root cloudera     736519 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-1.txt
-rw-r--r--   3 root cloudera     770324 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-2.txt
-rw-r--r--   3 root cloudera     843205 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-3.txt
-rw-r--r--   3 root cloudera     697960 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-4.txt
-rw-r--r--   3 root cloudera    3048008 2022-11-15 18:48 /user/cloudera/voyna-i-mir.txt

-- Теперь давайте изменим права доступа к нашему файлу. Чтобы с нашим файлом могли взаимодействовать коллеги, установите режим доступа, который дает полный доступ для владельца файла, а для сторонних пользователей возможность читать и выполнять.
root@6ab489fbd96a:/user/cloudera# hdfs dfs -chmod -R 764 /user/cloudera

--Попробуйте заново использовать команду для вывода содержимого папки и обратите внимание как изменились права доступа к файлу.
root@6ab489fbd96a:/user/cloudera# hdfs dfs -ls /user/cloudera
Found 5 items
-rwxrw-r--   3 root cloudera     736519 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-1.txt
-rwxrw-r--   3 root cloudera     770324 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-2.txt
-rwxrw-r--   3 root cloudera     843205 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-3.txt
-rwxrw-r--   3 root cloudera     697960 2022-11-15 18:41 /user/cloudera/voyna-i-mir-tom-4.txt
-rwxrw-r--   3 root cloudera    3048008 2022-11-15 18:48 /user/cloudera/voyna-i-mir.txt

--Теперь попробуем вывести на экран информацию о том, сколько места на диске занимает наш файл. Желательно, чтобы размер файла был удобночитаемым.
root@6ab489fbd96a:/user/cloudera# hdfs dfs -du -h /user/cloudera
719.3 K  /user/cloudera/voyna-i-mir-tom-1.txt
752.3 K  /user/cloudera/voyna-i-mir-tom-2.txt
823.4 K  /user/cloudera/voyna-i-mir-tom-3.txt
681.6 K  /user/cloudera/voyna-i-mir-tom-4.txt
2.9 M    /user/cloudera/voyna-i-mir.txt

-- На экране вы можете заметить 2 числа. Первое число – это фактический размер файла, а второе – это занимаемое файлом место на диске с учетом репликации. По умолчанию в данной версии HDFS эти числа будут одинаковы – это означает, что никакой репликации нет – нас это не очень устраивает, мы хотели бы, чтобы у наших файлов существовали резервные копии, поэтому напишите команду, которая изменит фактор репликации на 2.

--Повторите команду, которая выводит информацию о том, какое место на диске занимает файл и убедитесь, что изменения произошли.

--Напишите команду, которая подсчитывает количество строк в вашем файле

