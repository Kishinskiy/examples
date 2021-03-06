## Пример работы с mqtt сервером с использованием библиотеки paho.

[Сертификат удостоверяющего
центра](https://storage.yandexcloud.net/mqtt/rootCA.crt) включен в пример
строковым литералом.

Для работы примера нужно создать
[реестр](https://cloud.yandex.ru/docs/iot-core/quickstart#create-registry) и
[устройство](https://cloud.yandex.ru/docs/iot-core/quickstart#create-device).

Пример фактически делают эхо, то есть посланное в `$devices/<ID
устройства>/events` возвращается клиенту посредством подписки на этот топик
от имени реестра и выводится на консоль.

Поддерживаются два спосба
[авторизации](https://cloud.yandex.ru/docs/iot-core/concepts/authorization),
сертификаты и логин/пароль.


#### Сертификаты

В примере используются два
[сертификата](https://cloud.yandex.ru/docs/iot-core/quickstart#create-ca) - один
для устройства, один для реестра.

Расположение на диске:

    certs structure:
      /my_registry        Registry directory |currentDir|. Run samples from here.
      `- /device          Concrete device cert directory |device|.
      |  `- cert.pem
      |  `- key.pem
      |  `- keystore.p12  device certs pair in java friendly format
      `- cert.pem
      `- key.pem
      `- keystore.p12     registry certs pair in java friendly format

Для java примера .pem конвертируются в .p12:

    openssl pkcs12 -export -in cert.pem -inkey key.pem -out keystore.p12

Пример ищет сертификаты относительно current working directory, **поэтому
запускать их нужно в папке с сертификатами** (`my_registry` на схеме).


#### Логин/пароль

Нужно сгенерировать пароль для
[реестра](https://cloud.yandex.ru/docs/iot-core/operations/password/registry-password)
и для
[устройства](https://cloud.yandex.ru/docs/iot-core/operations/password/device-password).
Логины реестра и устройства это их `ID`.


### Особенности java.

java проект `pom.xml` использует maven, который сам находит зависимости.

Сборка:

    $ mvn install

Запуск:

    $ cd <my_registry>
    $ mvn exec:java -f <pom.xml path>


#### Сборка и запуск без maven.

[paho](https://www.eclipse.org/paho/clients/java/#) для java.

Сборка:

    $ javac -cp "<deps_dir>/org.eclipse.paho.client.mqtt.jar" "<exmple_java_path>/ExampleJava.java"

Запуск:

    $ java -cp "<deps_dir>/org.eclipse.paho.client.mqtt.jar" com.yandex.iot.examples.ExampleJava
