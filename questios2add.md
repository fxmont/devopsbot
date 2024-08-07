<details>
<summary>Какой CNI выбрать при установке Kubernetes? Чем отличаются?</summary>
При выборе CNI (Container Network Interface) для Kubernetes следует учитывать требования вашей среды и конкретные потребности. Некоторые из популярных CNI включают в себя:

Calico: Он обеспечивает богатую функциональность сети, включая сетевую политику и поддержку больших сетей. Он использует IP в качестве идентификаторов, что делает его хорошим выбором для крупных кластеров.
Flannel: Простой и легкий в настройке. Он обычно использует виртуальные сети в Layer 3.
Cilium: Обеспечивает широкий набор функций, включая маршрутизацию на уровне приложения, сетевую политику и безопасность. Он также интегрируется с другими инструментами, такими как Envoy и Istio.
Отличия между ними включают в себя производительность, функциональность, поддержку сетевой политики, поддержку различных сред и т. д. Лучший выбор зависит от ваших конкретных требований и предпочтений.

</details>
<details>
<summary>Как проводить установку через kube-spray? Какие компоненты стоит вынести и почему?</summary>
Kube-spray - это инструмент для развертывания Kubernetes кластеров. Установка через kube-spray включает в себя настройку различных компонентов Kubernetes, таких как API-серверы, контроллеры, рабочие узлы и сетевые ресурсы.

При установке через kube-spray стоит учитывать следующее:

Роли узлов: Вы можете выделить различные узлы для различных ролей Kubernetes, таких как мастер-ноды, рабочие узлы и узлы для специфических компонентов (например, узлы сети). Это позволяет лучше управлять ресурсами и обеспечивать отказоустойчивость.
Компоненты сети: Вы можете вынести компоненты сети, такие как CNI плагины, на отдельные узлы, чтобы обеспечить лучшую производительность сети и управление.
Хранение данных: Для компонентов, требующих хранение данных (например, etcd), рекомендуется выделить отдельные узлы или использовать внешние хранилища данных для обеспечения отказоустойчивости и производительности.
</details>
<details>
<summary>Если упираемся в версию kube-spray при обновлении кластера, то как в этом случае быть?</summary>
Если возникают проблемы с обновлением Kubernetes кластера из-за версии kube-spray, рекомендуется рассмотреть следующие шаги:

Проверьте актуальность версии kube-spray: Убедитесь, что вы используете последнюю стабильную версию kube-spray. Возможно, проблема уже была решена в новых версиях.
Изучите документацию и сообщества: Проверьте официальную документацию kube-spray и обсуждения в сообществах Kubernetes и kube-spray. Возможно, вы найдете решение или подсказку от других пользователей, которые столкнулись с подобной проблемой.
Попробуйте альтернативные методы обновления: Если обновление через kube-spray вызывает проблемы, рассмотрите возможность использования других инструментов или методов обновления Kubernetes, таких как kubeadm или управление через специализированные инструменты управления кластерами, такие как Rancher или kops.
Резервное копирование и восстановление: Прежде чем принимать решение о переходе на другой инструмент или метод обновления, убедитесь, что у вас есть надежное резервное копирование данных кластера, чтобы в случае неудачи можно было быстро восстановить кластер в рабочем состоянии.
</details>
<details>
<summary>Какие есть способы направить TCP\HTTP трафик в Kubernetes снаружи?</summary>
Для направления TCP/HTTP трафика в Kubernetes снаружи можно использовать различные способы:

Service Type LoadBalancer: Создание сервиса с типом LoadBalancer позволяет Kubernetes автоматически создать внешний балансировщик нагрузки (например, в облаке) и направить трафик на поды вашего сервиса.
Ingress Controller: Использование Ingress Controller позволяет настроить правила маршрутизации HTTP/S трафика на ваши сервисы внутри кластера. Ingress Controller может работать с различными реализациями, такими как Nginx Ingress Controller, Traefik, или Istio.
NodePort: Каждый сервис может быть доступен на каждой ноде кластера по определенному порту. Это может быть использовано для прямого доступа к сервису, хотя это не всегда рекомендуется из-за потенциальных проблем с безопасностью и управлением трафиком.
ExternalIPs: Вы можете назначить внешний IP-адрес на сервис, что позволит обращаться к сервису по этому IP-адресу снаружи кластера.
Ingress: Использование Ingress позволяет настроить правила маршрутизации для HTTP/S трафика на уровне приложения. Это более гибкий и мощный способ управления трафиком, чем простое использование LoadBalancer или NodePort.
Выбор конкретного способа зависит от требований вашего приложения, архитектуры среды и конфигурации сети.

</details>
<details>
<summary>Можно ли разместить в кластере несколько Ingress контроллеров? Какие в этом случае необходимо сделать изменения?</summary>
Да, можно разместить в кластере несколько Ingress контроллеров. Это может быть полезно, если у вас есть различные требования к маршрутизации трафика или если вы хотите использовать различные функции, предоставляемые разными Ingress контроллерами.

Чтобы разместить несколько Ingress контроллеров, необходимо выполнить следующие шаги:

Выбор контроллеров: Выберите различные реализации Ingress контроллеров, которые соответствуют вашим требованиям (например, Nginx Ingress Controller, Traefik, Istio Gateway и т. д.).
Конфигурация и развертывание: Разверните каждый выбранный Ingress контроллер в вашем кластере. Каждый из них должен быть настроен для обработки определенных запросов на основе ваших требований.
Настройка Ingress ресурсов: Создайте Ingress ресурсы для вашего приложения и определите, какой Ingress контроллер должен обрабатывать каждый ресурс. Это можно сделать с помощью аннотаций на уровне Ingress ресурсов.
Подключение к сети: Убедитесь, что все Ingress контроллеры имеют доступ к внешнему трафику и настроены правильно для приема трафика из интернета или внутри сети.
Обратите внимание, что использование нескольких Ingress контроллеров может добавить сложности в управлении сетью и маршрутизацией трафика, поэтому следует внимательно продумать их конфигурацию и развертывание.

</details>
<details>
<summary>Daemonset, statefulset, deployment чем различаются?</summary>
Deployment: Deployment обеспечивает декларативное обновление приложений в Kubernetes. Он управляет подами, предоставляя возможность создавать, обновлять и масштабировать приложения без прерывания работы. Deployment также предоставляет автоматическое восстановление при сбоях.
DaemonSet: DaemonSet гарантирует, что копия пода запущена на каждой ноде в кластере. Он часто используется для запуска служб, которые должны быть на каждой ноде, таких как сетевые или мониторинговые агенты.
StatefulSet: StatefulSet предназначен для приложений с состоянием, которые требуют уникальных, устойчивых и постоянных идентификаторов. Он обеспечивает управление жизненным циклом для каждого пода, сохраняя его состояние и обеспечивая уникальность идентификаторов.
Главное различие между ними заключается в том, как они управляют подами и их жизненным циклом. Deployment используется для обновления приложений без прерывания работы, DaemonSet для развертывания на каждой ноде и StatefulSet для приложений с состоянием, где требуется уникальность идентификаторов и управление жизненным циклом.

</details>
<details>
<summary>Какие лучшие практики по проектированию Kubernetes на 3 продукта, две среды. Одна из команд использует веб-сокет.</summary>
При проектировании Kubernetes на основе трех продуктов и двух сред рекомендуется учитывать следующие лучшие практики:

Микросервисная архитектура: Разбейте ваше приложение на микросервисы, каждый из которых отвечает за определенную функциональность. Это облегчит масштабирование и управление приложением.
Использование namespaces: Используйте namespaces для изоляции ресурсов и разделения окружений между продуктами и средами. Это обеспечит безопасность и изоляцию между компонентами.
Использование Ingress для доступа к сервисам: Настройте Ingress для каждого продукта и среды, чтобы обеспечить внешний доступ к сервисам. Учитывайте особенности использования веб-сокетов, если они используются в приложении.
Автоматизация развертывания и обновления: Используйте CI/CD pipelines для автоматизации развертывания и обновления приложений. Это обеспечит быстрое и надежное развертывание изменений.
Мониторинг и логирование: Настройте мониторинг и сбор логов для всех компонентов Kubernetes и приложений. Это поможет выявлять проблемы и улучшать производительность.
Управление конфигурациями: Используйте управление конфигурациями, такое как ConfigMaps и Secrets, для хранения конфигурационных данных приложений. Это обеспечит легкость управления и изменениями конфигурации.
Учитывайте специфику вашего приложения и требования к производительности, масштабируемости и безопасности при применении этих практик.

</details>
<details>
<summary>Где хранить стейт и файл блокировки с учетом использования подхода GitOPS?</summary>
При использовании подхода GitOps для управления инфраструктурой и приложениями в Kubernetes, хранение состояния (state) и файлов блокировки (lock files) может быть организовано следующим образом:

Хранение состояния Terraform: Для хранения состояния Terraform лучше всего использовать централизованное хранилище, такое как Amazon S3 bucket, Azure Blob Storage, Google Cloud Storage или любое другое подходящее хранилище объектов. Это обеспечит безопасное и надежное хранение состояния и доступ к нему из различных сред и команд.
Файлы блокировки: Файлы блокировки могут быть хранены вместе с кодом инфраструктуры в репозитории Git, аналогично другим файлам конфигурации. Это позволит вам отслеживать изменения в инфраструктуре и контролировать доступ к изменениям с помощью системы контроля версий.
При такой конфигурации важно обеспечить доступ к хранилищу состояния только для авторизованных пользователей или сервисов и обеспечить безопасность файлов блокировки, чтобы избежать конфликтов при одновременных обновлениях. Также рекомендуется регулярно резервировать состояние и регулярно проверять его целостность.

</details>
<details>
<summary>Необходимо создать 12 виртуальных машин однотипных для сервиса. Что использовать для автоматизации? IP задается статически. Маска для имени pkr-<service>-0*</summary>
Для автоматизации создания 12 виртуальных машин однотипных для сервиса, с использованием статических IP и заданной маски имени, можно воспользоваться инструментами автоматизации инфраструктуры, такими как Terraform.

Примерный шаги:

Определение конфигурации Terraform: Создайте конфигурационные файлы Terraform, в которых опишите требуемые ресурсы (виртуальные машины, сетевые интерфейсы, IP-адреса и т. д.), используя заданную маску имени для именования ресурсов.
Использование цикла для масштабирования: Воспользуйтесь циклом в Terraform для создания 12 экземпляров однотипных виртуальных машин, используя заданные статические IP-адреса и маску имени.
Настройка статических IP: Убедитесь, что вы настроили статические IP-адреса в соответствии с вашими требованиями, возможно, используя соответствующие провайдерские ресурсы в Terraform.
Применение конфигурации: Примените конфигурацию Terraform для создания и настройки виртуальных машин в вашем провайдере облачных услуг.
Убедитесь, что ваша конфигурация Terraform учитывает требования к сетевой конфигурации, безопасности и другие аспекты вашей инфраструктуры.

</details>
<details>
<summary>Повторим прошлый пункт, но добавляем условие, что 12 виртуалок - воркеры Kubernetes.</summary>
Для создания 12 виртуальных машин в качестве воркеров Kubernetes, с использованием статических IP и заданной маски имени, можно воспользоваться Terraform в сочетании с соответствующими инструментами для управления кластером Kubernetes, такими как kubeadm или kops.

Примерный шаги:

Определение конфигурации Terraform: Создайте конфигурационные файлы Terraform для создания виртуальных машин и настройки необходимых ресурсов с учетом требований Kubernetes.
Использование цикла для масштабирования: Воспользуйтесь циклом в Terraform для создания 12 экземпляров виртуальных машин, которые будут воркерами Kubernetes.
Настройка статических IP: Убедитесь, что вы настроили статические IP-адреса в соответствии с вашими требованиями, возможно, используя соответствующие провайдерские ресурсы в Terraform.
Применение конфигурации и развертывание Kubernetes: После создания виртуальных машин примените конфигурацию Terraform и используйте инструменты управления кластером Kubernetes, такие как kubeadm или kops, для их превращения в воркеры Kubernetes.
Убедитесь, что ваша конфигурация Terraform и настройки кластера Kubernetes соответствуют требованиям вашего приложения и обеспечивают требуемую производительность, масштабируемость и отказоустойчивость.

</details>
<details>
<summary>Как использовать IP адрес машины из Inventory в роли?</summary>
Для использования IP-адреса машины из Inventory в роли Ansible, вы можете воспользоваться встроенными переменными Ansible.

Пример использования IP-адреса машины из Inventory в роли:

yaml
Copy code
- name: Пример использования IP-адреса из Inventory
  hosts: ваша_группа_хостов
  tasks:
    - name: Вывести IP-адрес текущей машины
      debug:
        msg: "IP-адрес текущей машины: {{ ansible_host }}"
Переменная ansible_host содержит IP-адрес машины, на которой выполняется роль. Вы можете использовать эту переменную в своих задачах для обращения к машинам по IP-адресу.

</details>
<details>
<summary>Как написать роль, когда процедура обновления и установки различаются между собой?</summary>
Если процедура обновления и установки различаются между собой в роли Ansible, вы можете разделить их на отдельные задачи или использовать условные конструкции для определения, какую процедуру следует выполнить.

Пример написания роли с различными процедурами обновления и установки:

yaml
Copy code
- name: Установка или обновление пакета
  hosts: ваша_группа_хостов
  tasks:
    - name: Проверить наличие пакета
      stat:
        path: /path/to/package
      register: package_check

    - name: Установить пакет, если его нет
      package:
        name: ваш_пакет
        state: present
      when: not package_check.stat.exists

    - name: Обновить пакет, если он установлен
      package:
        name: ваш_пакет
        state: latest
      when: package_check.stat.exists
В этом примере используется условная конструкция when, чтобы определить, должна ли выполняться установка или обновление пакета в зависимости от его наличия на целевой машине. При необходимости вы можете дополнить этот пример дополнительными задачами или условиями в соответствии с вашими требованиями.

</details>
<details>
<summary>Как провести обновление сервиса, когда необходимо в начале обновить мастер, а потом другие ноды (мастер выбирается динамически)</summary>
Для проведения обновления сервиса в сценарии, когда сначала необходимо обновить мастер, а затем другие ноды, и при этом мастер выбирается динамически, вы можете воспользоваться следующим подходом:

Определите динамический выбор мастера: Используйте факты Ansible или другие механизмы для динамического определения текущего мастера в кластере.
Выделите обновление мастера: Создайте задачи Ansible для обновления мастера. Это может включать в себя остановку мастера, обновление его компонентов и повторный запуск.
Выделите обновление других нод: Создайте отдельные задачи для обновления других нод кластера после обновления мастера. Обновление других нод может включать в себя их остановку, обновление компонентов и повторный запуск.
Управление последовательностью выполнения: Убедитесь, что задачи для обновления мастера выполняются до задач для обновления других нод. Это можно сделать с помощью определения порядка выполнения задач или использования тегов для группировки задач.
Тестирование и валидация: После обновления мастера и других нод убедитесь, что кластер работает корректно и сервисы функционируют нормально.
Примените этот подход к вашей роли Ansible, учитывая специфику вашего кластера Kubernetes и требования вашего сервиса. Важно также учесть любые особенности вашего окружения и возможные риски при проведении обновления.
</details>
