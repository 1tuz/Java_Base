#solid #паттерны
SOLID – это принципы разработки программного обеспечения, следуя которым получаем хороший код, который в дальнейшем будет хорошо масштабироваться и поддерживаться в рабочем состоянии.


Single Responsibility Principle – принцип единственной ответственности. Каждый класс должен иметь только одну зону ответственности.

Open closed Principle – принцип открытости-закрытости. Классы должны быть открыты для расширения, но закрыты для изменения.


Liskov substitution Principle – принцип подстановки Барбары Лисков. Должна быть возможность вместо базового (родительского) типа класса подставить любой его подтип (класс-наследник), при этом работа программы не должна измениться. Как запомнить? Поведение потомка не должно противоречить поведению родителя.

Interface Segregation Principle – это принцип разделения интерфейсов. Данный принцип обозначает, что не нужно заставлять клиента (класс) реализовывать интерфейс, который не имеет к нему отношения.

Dependency Inversion Principle – это принцип инверсии зависимостей.  Абстракции НЕ должны зависеть от деталей. Детали должны зависеть от абстракций. Модули верхнего уровня НЕ должны зависеть от модулей нижнего уровня, НО должны зависеть от абстракции. Геометрическая фигура – абстракция. Круг, квадрат – детали.