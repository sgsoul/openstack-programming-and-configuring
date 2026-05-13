# Лабораторная работа №2 Создание ВМ

<details>
<summary> Definition of Done </summary>

1. Созданы публичные и приватные сети Neutron
2. Создан и присвоен флейвор Glance
3. Создано и подключено блочное устройство Cinder
4. Создан рабочий инстанс Nova

</details>

## Работа с сетями Neutron

Создается публичная сеть провайдера:

<img width="1422" height="574" alt="image" src="https://github.com/user-attachments/assets/76c61aec-9b7c-4b4b-ac09-9b6de90149e2" />

Задаются переменные окружения `IP` и `baseIP`:

<img width="1369" height="282" alt="image" src="https://github.com/user-attachments/assets/044c8e66-645c-44a7-a318-60b4eab4395b" />

Создается подсеть в публичной сети провайдера: 

<img width="1400" height="499" alt="image" src="https://github.com/user-attachments/assets/4fabaed6-77cf-42db-aa6b-1c8738f06362" />

Проверятся список сетей и подсетей:

<img width="1379" height="307" alt="image" src="https://github.com/user-attachments/assets/3317a78f-418e-43b6-b62f-98813e136297" />

Создается локальная сеть:

<img width="1355" height="788" alt="image" src="https://github.com/user-attachments/assets/525a9b21-78cd-495e-ac00-4f47467b1971" />

Создается подсеть платформы OpenStack для выделения адресов ВМ:

<img width="1410" height="478" alt="image" src="https://github.com/user-attachments/assets/0d6f5718-07c2-4d6b-9ce9-55741efa3eb2" />

Проверятся список сетей и подсетей:

<img width="1413" height="374" alt="image" src="https://github.com/user-attachments/assets/3022cbfb-00cf-44cc-896a-1529787cc17c" />

Создается роутер и подключаются созданные сети:

<img width="1357" height="685" alt="image" src="https://github.com/user-attachments/assets/1f8993f5-f663-4827-9bce-9a6496ecbaa9" />

<img width="1155" height="579" alt="image" src="https://github.com/user-attachments/assets/8da89fe9-62a2-44be-bb49-9a2e1029fae8" />

## Работа с Glance, SSH, Cinder

Создается флейвор виртуальной машины. При запуске ВМ необходимо выбрать её размер - будет задано ограничение по оперативной памяти в 256МБ:

<img width="1122" height="449" alt="image" src="https://github.com/user-attachments/assets/96eb1e84-08c7-4853-b9ae-7e07b9836b32" />

Создается ключ доступа для работы с будущими ВМ:

<img width="1154" height="314" alt="image" src="https://github.com/user-attachments/assets/37f81d2d-147e-4e29-9026-1932958edada" />

Загружается образ Cirros в OpenStack Glance. Образ скачивается с официального сайта и обладает минимальным набором пакетов:

<img width="1153" height="564" alt="image" src="https://github.com/user-attachments/assets/1320bda8-1a63-4acc-80bc-981dc5d14dd2" />

<img width="1155" height="305" alt="image" src="https://github.com/user-attachments/assets/909597b7-b879-4104-990f-7455897b79c0" />


Создается блочное устройство в Cinder, где в качестве истчоника используется ранее загруженный образ Cirros:

<img width="1152" height="500" alt="image" src="https://github.com/user-attachments/assets/26457a09-3838-4d97-89aa-fd6823b51f52" />

## Работа с инстансами Nova в UI

В панели Horizon и создается виртуальная машина на созданных ранее ресурсах:

<img width="1645" height="551" alt="image" src="https://github.com/user-attachments/assets/0563665e-9bd8-40e2-a164-999bffa37250" />

<details>
<summary> Из-за проблемы сетевой связности [host-VMware-instance] доступ к консоли не отрабатывает должным образом: </summary>

Получить доступ к ВМ через SSH так же не получилось, потому что и инстанс, и ВМ для OpenStack собирались на минимальном образе. Скорее всего проблема в прокидывании доступа к "внутренней" ВМ через ВМ-OpenStack'a.
Для отладки я бы проверила связность при установке OpenStack на bare-metal, либо при устанвоке сервиса на ВМ с графикой, чтобы проверить доступ консоли, через встроенный браузер

<img width="1085" height="537" alt="image" src="https://github.com/user-attachments/assets/49533597-a4d2-4694-ab1b-e189f4009428" />
</details>

<img width="1650" height="209" alt="image" src="https://github.com/user-attachments/assets/0b39df51-479a-437c-a09c-59911c0d0ae8" />

Однако, по логам созданной ВМ видно, что сетевая настройка прошла успешно и инстанс работоспособен:

<img width="854" height="755" alt="image" src="https://github.com/user-attachments/assets/48e3b4b1-0cfd-46e2-9797-eae6e5fca7c4" />

<img width="1233" height="298" alt="image" src="https://github.com/user-attachments/assets/e530997e-ae70-468f-acc8-ab785fc8f097" />

## Работа с сетями через UI 

Через Horizon создается еще одну приватная сеть и её подсеть:

<img width="1582" height="521" alt="image" src="https://github.com/user-attachments/assets/4284f3b8-3e0d-4a3d-8cfd-10aac65dc552" />

Сети подключаются к уже существующему роутеру:

<img width="1575" height="533" alt="image" src="https://github.com/user-attachments/assets/7afd8aba-ed14-44c4-b084-3fe0e0437604" />

<img width="1085" height="699" alt="image" src="https://github.com/user-attachments/assets/3dff7fc6-71e3-4dfc-ab3a-f241181dc38e" />

## Создание ВМ через CLI

Через Openstack CLI создается копию ранее созданного блочного устройства:

<img width="1281" height="810" alt="image" src="https://github.com/user-attachments/assets/57d7d0e0-b943-4845-b301-cc0b552d38f5" />

Через Openstack CLI создается новая ВМ на новосозданных ресурсах:

> предварительно в переменные окружения кладется значение ID приватной подсети
> 
> <img width="1458" height="387" alt="image" src="https://github.com/user-attachments/assets/61cb2519-184f-4cab-aa9b-78a2ac05ab7e" />

<img width="1821" height="814" alt="image" src="https://github.com/user-attachments/assets/f9ac81c5-7c60-4bc1-bd42-ac3c243f6c3a" />

В панели Horizon проверяется статус созданного инстанса:

<img width="1778" height="578" alt="image" src="https://github.com/user-attachments/assets/f49d4a77-a713-4c42-a8bd-ba89cc625bbb" />


## Вопросы

<details>
<summary> 1. Что именно сервис с помощью Keystone проверяет в токене пользователя, когда тот пытается осуществить операцию по отношению к этому сервису? </summary>

</details>

<details>
<summary> 2. При создании ВМ, Nova первым делом идет в Keystone, проверяет токен и т.д. Как думаете, к эндпоинту какого сервиса Nova идет следом? </summary>

</details>
