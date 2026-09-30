# Домашнее задание. Занятие 11. TeamCity

Репозиторий: https://github.com/ryltsevavikyla-coder/example-teamcity

## Подготовка

- TeamCity Server: http://111.88.145.198:8111  
  ВМ 4 CPU / 4 RAM, образ jetbrains/teamcity-server, первичная настройка выполнена.
- TeamCity Agent: 158.160.76.93  
  ВМ 2 CPU / 4 RAM, SERVER_URL=http://111.88.145.198:8111, агент авторизован.
- Nexus: http://111.88.148.60:8081  
  ВМ 2 CPU / 4 RAM, playbook из infrastructure запущен.

## Основная часть

1. Проект в TeamCity создан из форка, включён autodetect Maven.
2. Первая сборка master прошла успешно.
3. Условия шагов:
   - ветка не master → `mvn clean test`
   - ветка master / default → `mvn clean deploy`
4. В TeamCity загружен `settings.xml` с учётками Nexus (`admin` / `admin123`).
5. В `pom.xml` указан свой Nexus:
   `http://111.88.148.60:8081/repository/maven-releases/`
6. Сборка master задеплоила артефакт `plaindoll` 0.0.3 в Nexus.
7. Конфигурация перенесена в репозиторий (Versioned Settings, Kotlin).
   Файл: `.teamcity/settings.kts`
8. Ветка `feature/add_reply`:
   - метод `sayHunter()` в классе `Welcomer`
   - тест `newReplyContainsHunter` на слово `hunter`
   - сборка ветки прошла, тесты зелёные
9. Ветка влита в master через Pull Request.
10. До настройки Artifact paths у сборки master артефактов не было.
11. Добавлен Artifact path `target/*.jar`.
    Повторная сборка master опубликовала `plaindoll-0.0.3.jar`.
12. В `.teamcity/settings.kts` есть шаги Maven, условия веток и `artifactRules`.
