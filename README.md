bastion_IP = 35.204.11.162

someinternalhost_IP = 10.164.0.3
'''
gcloud compute instances create reddit-app-2 \
  --boot-disk-size=10GB \
  --image-family ubuntu-1604-lts \
  --image-project=ubuntu-os-cloud \
  --machine-type=g1-small \
  --tags puma-server \
  --restart-on-failure \
  --metadata-from-file startup-script= /mnt/c/users/user/documents/otus/task\ 4/vlasovvo_infra/startup-script.sh
'''
'''
gcloud compute firewall-rules create default-puma-server\
 --rules=tcp:9393 \
 --source-ranges=0.0.0.0/0 \
 --target-tags=puma-server
'''
testapp_IP= 35.204.231.87

testapp_port = 9292

## HW#5

Выполнены основные задания и со *

## HW#6

### Задание со * :

 - Может быть только 1 **google_compute_project_metadata_item** c ключем **ssh-keys**. В противном случае значения будут перезаписывать друг друга.
 - Написание :\
 '''
   resource "google_compute_project_metadata_item" "add_many_user" {\
   count = 4\
   key = "ssh-keys"\
   value = "appuser-${count.index + 1}:${file("\~/.ssh/appuser.pub")}\
 '''
  Не эквивалентно:\
  '''
   resource "google_compute_project_metadata_item" "add_many_user" {\
   key = "ssh-keys"\
   value = "appuser1:${file("\~/.ssh/appuser.pub")}appuser2:${file(\~/.ssh/appuser.pub")}appuser3:${file("\~/.ssh/appuser.pub")}appuser4:${file("\~/.ssh/appuser.pub")}"\
   '''
  В первом случае создается 4 **google_compute_project_metadata_item**, которые перезаписывают друг друга.
 - При добавлении ssh ключа через web интерфейс и выполнении команды **terraform** **apply** ssh ключ будет удален, т.к. его описание не описано в файле **main.tf**
### Заданиие ** :
 В обоих инстансах своя БД

##HW#7

### Задание со *:

 - При одновременном вызове команд terraform второе выполнение блокируется и возвращается данная ошибка:

 >Error: Error locking state: Error acquiring the state lock: writing "gs://storage-bucket-vvo1/terraform/stage/default.tflock" failed: googleapi: Error 412: Precondition Failed, conditionNotMet
 - Не теста ради, по случайности потерялось интернет соединение при выполнении команды **terraform** **apply**. В итоге не выполнившись до конца команда в GCS остался lock файл, который не позволял что либо сделать. Лечилось ручным удалением инстансов и ресурсов в GCP, повторным выполнением **terraform** **apply** c флагом **lock=false** и удалнием lock файла из GCS. 
