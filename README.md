"# Lesson3.1_HDFS" 

-- После того, как файлы окажутся на HDFS попробуйте выполнить команду, которая выводит содержимое папки. Особенно обратите внимание на права доступа к вашим файлам.
root@6ab489fbd96a:/# hadoop fs -ls /user/cloudera
Found 4 items
-rw-r--r--   3 cloudera cloudera     736519 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-1.txt
-rw-r--r--   3 cloudera cloudera     770324 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-2.txt
-rw-r--r--   3 cloudera cloudera     843205 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-3.txt
-rw-r--r--   3 cloudera cloudera     697960 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-4.txt

-- Далее сожмите все 4 тома в 1 файл.
??
-- Теперь давайте изменим права доступа к нашему файлу. Чтобы с нашим файлом могли взаимодействовать коллеги, установите режим доступа, который дает полный доступ для владельца файла, а для сторонних пользователей возможность читать и выполнять.
root@6ab489fbd96a:/# hadoop fs -chmod -R 755 /user/cloudera/

--Попробуйте заново использовать команду для вывода содержимого папки и обратите внимание как изменились права доступа к файлу.
root@6ab489fbd96a:/# hadoop fs -ls /user/cloudera/
Found 4 items
-rwxr-xr-x   3 cloudera cloudera     736519 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-1.txt
-rwxr-xr-x   3 cloudera cloudera     770324 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-2.txt
-rwxr-xr-x   3 cloudera cloudera     843205 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-3.txt
-rwxr-xr-x   3 cloudera cloudera     697960 2022-11-14 19:31 /user/cloudera/voyna-i-mir-tom-4.txt

--Теперь попробуем вывести на экран информацию о том, сколько места на диске занимает наш файл. Желательно, чтобы размер файла был удобночитаемым.
root@6ab489fbd96a:/# hadoop fs -du -h /user/cloudera/
719.3 K  /user/cloudera/voyna-i-mir-tom-1.txt
752.3 K  /user/cloudera/voyna-i-mir-tom-2.txt
823.4 K  /user/cloudera/voyna-i-mir-tom-3.txt
681.6 K  /user/cloudera/voyna-i-mir-tom-4.txt

-- На экране вы можете заметить 2 числа. Первое число – это фактический размер файла, а второе – это занимаемое файлом место на диске с учетом репликации. По умолчанию в данной версии HDFS эти числа будут одинаковы – это означает, что никакой репликации нет – нас это не очень устраивает, мы хотели бы, чтобы у наших файлов существовали резервные копии, поэтому напишите команду, которая изменит фактор репликации на 2.

--Повторите команду, которая выводит информацию о том, какое место на диске занимает файл и убедитесь, что изменения произошли.

--Напишите команду, которая подсчитывает количество строк в вашем файле

