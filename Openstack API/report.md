# Лабораторная работа №3 Работа с Openstack API

<details>
<summary> Definition of Done </summary>

1. Познакомились с Openstack API
2. Научились авторизовываться по паролю и токену
3. Научились управлять ресурсами Openstack через эндпоинты

</details>

## Работа с Keystone

У Keystone запрашивается токен для дальнейшей работы:

<img width="1647" height="743" alt="image" src="https://github.com/user-attachments/assets/ef96ee38-84d1-4bcd-a771-7132a6e1535f" />

Проверяется, что токен рабочий - запрашивается эндпоинт для получения списка проектов:

<img width="1820" height="470" alt="image" src="https://github.com/user-attachments/assets/ceac5b94-4538-4309-8cbe-a5519f826fbd" />

Через Nova API создается новый рабочий инстанс:

> для этого сначала узнаются ID необходимых ресурсов:
> <img width="1083" height="749" alt="image" src="https://github.com/user-attachments/assets/7510b0b6-7a65-44d3-a770-44b499154851" />

<img width="1504" height="515" alt="image" src="https://github.com/user-attachments/assets/49929130-5525-4d05-b8fc-2cca3e405f59" />

<img width="1658" height="551" alt="image" src="https://github.com/user-attachments/assets/dfc1bd88-0d82-4c01-a3e8-5849ab2ac143" />

## Подключение Ceph

> в силу свободных 30ГБ на хосте, было принято грустное волевое решение ставить ceph на узел с OpenStack и поднимать всего 1 диск =( потенциал.не раскрыт

Установливается [`cephadm`](https://docs.ceph.com/en/latest/cephadm/install/):
<img width="864" height="525" alt="image" src="https://github.com/user-attachments/assets/aac224c9-0dd4-4e0a-bbbd-332eeef88925" />

<img width="1328" height="196" alt="image" src="https://github.com/user-attachments/assets/cd4acfa5-1cb0-41b5-a381-bb9acc298ffb" />

Ставятся необходимые пакеты и утилита ceph-common. Подключается репо версии `Squid`:
<img width="1317" height="638" alt="image" src="https://github.com/user-attachments/assets/9f3363a9-c283-4863-a647-a914fabc7f0f" />

<img width="1791" height="428" alt="image" src="https://github.com/user-attachments/assets/96742884-ec47-43b4-825c-bb980abb99ca" />

В конфигурации прописываются fsid и ключ для mon компоненты, где указываются демон монитора (наш хост) и параметры репликации:

<img width="1030" height="427" alt="image" src="https://github.com/user-attachments/assets/4907033b-8846-4361-a7e0-4b457da98b5c" />

Генерируется ключ для монитора:

<img width="1688" height="90" alt="image" src="https://github.com/user-attachments/assets/2d7638bb-44d1-4436-979d-9d856a5fd247" />

Генерируется ключ для администратора, после чего он импортируется в keyring монитора:

<img width="1678" height="379" alt="image" src="https://github.com/user-attachments/assets/3f989fc5-cada-43c6-bd0b-d6c0d8a8477e" />

==============================================================================================================================================>

Подготавливается директория монитора и файлового хранилища mon:

<img width="1633" height="236" alt="image" src="https://github.com/user-attachments/assets/44adc813-5dd9-41db-8518-a26ab634fb75" />


Проверятся статус кластера Ceph:

<img width="1153" height="529" alt="image" src="https://github.com/user-attachments/assets/a4535a91-234b-49b7-933e-b4989b9716fe" />

lvm /dev/sda

<img width="1596" height="583" alt="image" src="https://github.com/user-attachments/assets/d430770a-0b94-40c4-b894-a8ff6dfe8dd7" />
<img width="1607" height="351" alt="image" src="https://github.com/user-attachments/assets/7f8f5e80-d8e3-40be-a663-8132f47d9845" />


Проверятся статус кластера Ceph:

<img width="1578" height="735" alt="image" src="https://github.com/user-attachments/assets/c20fc4eb-d3f1-4a5a-83a6-71ef8a1eb53f" />


osd pool && auth

<img width="1577" height="592" alt="image" src="https://github.com/user-attachments/assets/ea600040-cf11-429f-9f91-bc54048ed672" />

OS cinder conf

<img width="1164" height="295" alt="image" src="https://github.com/user-attachments/assets/dbcbc055-2464-4049-a5db-914208926dd9" />
<img width="1302" height="471" alt="image" src="https://github.com/user-attachments/assets/2bf19163-7715-4c73-aa69-98c079fcc687" />


restart cinder comps

<img width="1339" height="172" alt="image" src="https://github.com/user-attachments/assets/f453ac57-cf6b-430c-90b8-e2e9f95a6234" />
<img width="1806" height="251" alt="image" src="https://github.com/user-attachments/assets/50ebc995-dd58-4a9a-8eb5-22929071890a" />
<img width="1823" height="877" alt="image" src="https://github.com/user-attachments/assets/ae0008f4-9904-491b-9173-f7611997c10b" />

os volume type

<img width="1805" height="529" alt="image" src="https://github.com/user-attachments/assets/622fb3a8-4157-431a-8881-8d3af7248300" />


os volume list ceph rbd

<img width="1230" height="841" alt="image" src="https://github.com/user-attachments/assets/734692ef-5cd7-4afc-8056-83384127b35a" />

<img width="1646" height="710" alt="image" src="https://github.com/user-attachments/assets/e39d25a2-cfbe-40a9-8763-f127366f9cea" />

ceph osd loops

<img width="837" height="139" alt="image" src="https://github.com/user-attachments/assets/96ee926a-a38c-46cb-8ae4-090feb6d051d" />
