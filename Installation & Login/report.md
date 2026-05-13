# Лабораторная работа №1 Подготовка к развертыванию OpenStack

<details>
<summary> Definition of Done </summary>

1. Развернута ВМ на базе образа Alma Linux 9.3
2. Настройка среды для установки Openstack
3. Установка OpenStack
4. Изучены возможности OpenStack CLI
5. Вход в Openstack через Horizon
6. Изучены способы создания проектов и пользователей

</details>

## Создание ВМ
Создается ВМ в VMware на базе образа Alma Linux 9.3 со следующими характеристиками:

<img width="464" height="508" alt="image" src="https://github.com/user-attachments/assets/2120f0a9-e734-44ee-9942-3494ac51adb8" />


Проверяется, что всё установилось корректно, сетевые настройки работают: 

<img width="1606" height="719" alt="image" src="https://github.com/user-attachments/assets/2513e52a-0d63-4b77-9e74-8d607b9df7bd" style="width: 90%;"/>


## Создание проекта
Клонируется [проект](https://gitlab.com/itmo_samon/openstack_lab) для установки OpenStack:

<img width="1281" height="362" alt="image" src="https://github.com/user-attachments/assets/e3577296-9524-4c20-a286-3932298dd90b" style="width: 60%;"/>

В склонированном проекте запускается сркипт `./prepare.sh`.

Скрипт отключает firewalld и SELinux, устанавливает пакеты OpenStack, при необходимости добавляет SWAP до 12ГБ.

<img width="1026" height="370" alt="image" src="https://github.com/user-attachments/assets/f8e42899-fae2-426e-b82c-aa6a59766a20" style="width: 60%;"/>

После этого запускается `./config.sh`.

Скрипт конфигурирует `./answer.cfg` для OpenStack: отключает компоненты Swift, Ceilometer, Aodh, metering, demo. Задает cinder volumes в 10GB и число воркеров =1, настраивает сетевой адаптер.

<img width="1069" height="301" alt="image" src="https://github.com/user-attachments/assets/3aea04ec-997e-4c15-89c3-cf5338c8eb7e" style="width: 60%;"/>

Установливается OpenStack: `packstack --answer-file=answer.cfg`

<details>
<summary> Первая попытка </summary>

На первой попытке установка упадет на SSH таймауте, не поставив часть сервисов, но об этом станет известно только во 2ой ЛР. :') После увелечения ядер на ВМ до 4х, всё проедет успешно.
  
<img width="1154" height="706" alt="image" src="https://github.com/user-attachments/assets/ca8f18e0-f63f-415c-8b2e-17a091fd2882" style="width: 90%;"/>

```bash
[root@192 ~(keystone_admin)]# openstack endpoint list
+----------------------------------+-----------+--------------+--------------+---------+-----------+----------------------------+
| ID                               | Region    | Service Name | Service Type | Enabled | Interface | URL                        |
+----------------------------------+-----------+--------------+--------------+---------+-----------+----------------------------+
| 31959e45315f404c9588d688b9e99ec6 | RegionOne | keystone     | identity     | True    | public    | http://192.168.11.130:5000 |
| 8851fdf252394c97a809afffdefffb98 | RegionOne | keystone     | identity     | True    | admin     | http://192.168.11.130:5000 |
| a38b6c1f629b40f59c3470970664a77e | RegionOne | keystone     | identity     | True    | internal  | http://192.168.11.130:5000 |
+----------------------------------+-----------+--------------+--------------+---------+-----------+----------------------------+
[root@192 ~(keystone_admin)]# openstack user list
+----------------------------------+-------+
| ID                               | Name  |
+----------------------------------+-------+
| c755318c4ede41cd924a548ef7069e3d | admin |
+----------------------------------+-------+
[root@192 ~(keystone_admin)]# openstack project list
+----------------------------------+-------+
| ID                               | Name  |
+----------------------------------+-------+
| ff0561e3eae34726be671d9bcec83b68 | admin |
+----------------------------------+-------+
[root@192 ~(keystone_admin)]#
```

</details>

<img width="1168" height="665" alt="image" src="https://github.com/user-attachments/assets/afe72cc1-3b6f-418b-bb60-cadd4fb031e3" style="width: 90%;" />

После успешной установки будут доступы данные от УЗ админа: 

<img width="1160" height="514" alt="image" src="https://github.com/user-attachments/assets/a80bf158-1fdb-4586-a7d0-4b72663e7029" style="width: 60%;" />

Проверяются установленные сервисы, а так же пользователи и проекты, созданные для этих сервисов:

> тк переустановка была выполнена во 2ЛР, в выводе также присутсвуют проекты и пользователи, созданные в процессе выполнения 1ЛР
```bash
[root@controller ~(keystone_admin)]# openstack endpoint list
+----------------------------------+-----------+--------------+--------------+---------+-----------+---------------------------------+
| ID                               | Region    | Service Name | Service Type | Enabled | Interface | URL                             |
+----------------------------------+-----------+--------------+--------------+---------+-----------+---------------------------------+
| 0d7604f468774a7abf5e185857c777f9 | RegionOne | placement    | placement    | True    | internal  | http://192.168.11.130:8778      |
| 1868ae6997044037832fccc77f7f6df0 | RegionOne | cinderv3     | volumev3     | True    | public    | http://192.168.11.130:8776/v3   |
| 292b8ad6280a41379b4b08cbac229593 | RegionOne | cinderv3     | volumev3     | True    | admin     | http://192.168.11.130:8776/v3   |
| 31959e45315f404c9588d688b9e99ec6 | RegionOne | keystone     | identity     | True    | public    | http://192.168.11.130:5000      |
| 38ac3b1ac6274574a27435e7746cc084 | RegionOne | neutron      | network      | True    | public    | http://192.168.11.130:9696      |
| 5be61d3c5f504d3a9c1cf66f26896514 | RegionOne | nova         | compute      | True    | admin     | http://192.168.11.130:8774/v2.1 |
| 637f328b0db14b1a81fc1a8b5bcf270a | RegionOne | placement    | placement    | True    | admin     | http://192.168.11.130:8778      |
| 6dbe46ed648b472fb45b0fa642f7fe0b | RegionOne | neutron      | network      | True    | admin     | http://192.168.11.130:9696      |
| 7aef5a6b2b704da4beeb1818f6cfc2be | RegionOne | glance       | image        | True    | admin     | http://192.168.11.130:9292      |
| 8851fdf252394c97a809afffdefffb98 | RegionOne | keystone     | identity     | True    | admin     | http://192.168.11.130:5000      |
| a38b6c1f629b40f59c3470970664a77e | RegionOne | keystone     | identity     | True    | internal  | http://192.168.11.130:5000      |
| ac61076f8c8a406b848b162e4ba423d4 | RegionOne | glance       | image        | True    | public    | http://192.168.11.130:9292      |
| b104acd0221f438fafb67918d509faef | RegionOne | placement    | placement    | True    | public    | http://192.168.11.130:8778      |
| b991d418d65742a9b0abdf21b157f0c2 | RegionOne | neutron      | network      | True    | internal  | http://192.168.11.130:9696      |
| c56984bd25d94bf4b695af425e6cec38 | RegionOne | nova         | compute      | True    | public    | http://192.168.11.130:8774/v2.1 |
| d68678c151d64631971853b288d083d0 | RegionOne | glance       | image        | True    | internal  | http://192.168.11.130:9292      |
| e76ba028cca54ba7a998c35efaaebb7a | RegionOne | nova         | compute      | True    | internal  | http://192.168.11.130:8774/v2.1 |
| ef34d803d8314a24b295b1e6efaf9a0b | RegionOne | cinderv3     | volumev3     | True    | internal  | http://192.168.11.130:8776/v3   |
+----------------------------------+-----------+--------------+--------------+---------+-----------+---------------------------------+
[root@controller ~(keystone_admin)]# openstack user list
+----------------------------------+------------+
| ID                               | Name       |
+----------------------------------+------------+
| c755318c4ede41cd924a548ef7069e3d | admin      |
| df7b938756ef4fb782511e3cf8376e8d | iloskutova |
| 20243ee024b9426395e7f8b23a90eae4 | glance     |
| c142d8dd202d4f8cb6d6a4954791dc9e | cinder     |
| 8a0e5e75512a453e9af61571e9e32ecd | nova       |
| f8f8d1e37bd94b06b1f851a95fa1e20b | placement  |
| 99b3decfa05341368ee4b1ad1e72c9d9 | neutron    |
+----------------------------------+------------+
[root@controller ~(keystone_admin)]# openstack project list
+----------------------------------+-------------+
| ID                               | Name        |
+----------------------------------+-------------+
| 0807ad77025345619c4e9a8b14e87464 | services    |
| cb952f80119e4bc9a873b38ea56da6d4 | lab-project |
| df12709da5024f0a8a6385c401ada64d | demo        |
| ff0561e3eae34726be671d9bcec83b68 | admin       |
+----------------------------------+-------------+
[root@controller ~(keystone_admin)]#
```

Для проверки корректности установки проверяется работоспособность Keystone - Identity service OpenStack, который отвечает за токены, пользователей и доступ к другим компонентам.

<img width="566" height="500" alt="image" src="https://github.com/user-attachments/assets/04c0f2fa-1d0d-41f8-9012-fee74b09f9a6" style="width: 60%;" />

## Работа с OpenStack CLI/UI

> далее работа выполнятся в 1ой версии установки OpenStack, поэтому часть (вообще все) проектов в выводе отсутсвует

С помощью CLI создается демо-проект:

<img width="1139" height="689" alt="image" src="https://github.com/user-attachments/assets/18ba4b96-d246-4583-b256-97f1246ffca6" style="width: 90%;" />

Выполняется авторизация в Horizon под УЗ админа:

<img width="1666" height="541" alt="image" src="https://github.com/user-attachments/assets/d42c1994-febc-4538-9f55-8dcca1e4a5a7" style="width: 100%;" />

В UI создается новый пользователь `iloskutova`:

<img width="1546" height="500" alt="image" src="https://github.com/user-attachments/assets/a1b0f62e-5aa8-475b-aec9-c0ff6fe8ef1b" style="width: 100%;" />

При попытке создать новый проект высвечивалсь ошибка о том, что необходимая дефолтная роль для пользователя отсутвует:

<img width="1553" height="443" alt="image" src="https://github.com/user-attachments/assets/45e92249-add1-4d30-acba-891351f12e26" style="width: 100%;" />

Поэтому пришлось вручную добавить роль `__member__` - это стандартная роль для пользователей, привязанных к проекту:

<img width="923" height="672" alt="image" src="https://github.com/user-attachments/assets/d14d3850-9491-4411-ab1f-aa0e30d995e2" style="width: 60%;" />

После этого новый проект создался успешно:

<img width="1483" height="467" alt="image" src="https://github.com/user-attachments/assets/195aba2e-82cc-4973-b449-05c61fc69d03" style="width: 100%;" />

В проект добавляются пользователи `iloskutova` и `admin` с необходимыми уровнями доступа:

<img width="1475" height="631" alt="image" src="https://github.com/user-attachments/assets/41a31448-3d83-4d9f-8ab4-5374fddc9355" style="width: 70%;" />

После чего выполнятся авторизация под новым пользователем и проверяется доступ к проекту:

<img width="1481" height="378" alt="image" src="https://github.com/user-attachments/assets/bb1316c0-2fd5-48c3-ae26-8318629207ce" style="width: 100%;" />



