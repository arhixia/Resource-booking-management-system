Этот проект представляет собой систему управления бронированиями ресурсов, таких как столики в ресторане или конференц-залы. В системе реализованы функциональные возможности для регистрации пользователей, управления ресурсами, создания расписаний и бронирований, а также для проверки и подтверждения этих бронирований. Проект использует FastAPI для создания RESTful API, SQLAlchemy для работы с базой данных PostgreSQL, и JWT для аутентификации и авторизации пользователей.

Основные компоненты и их функциональность


<h1> <strong> users.py: </strong> </h1>

Регистрация пользователя (/users/register): Позволяет пользователям создавать новые учетные записи. Если имя пользователя уже занято, возвращает ошибку.

Аутентификация (/users/token): Выдает JWT токен для аутентифицированного пользователя. Требует имя пользователя и пароль.

Получение текущего пользователя: Используется для проверки аутентификации в других эндпоинтах.

Проверка прав администратора (verify_admin): Проверяет, является ли текущий пользователь администратором (в данном случае, пользователь с id = 1).

Логаут (/users/logout): Добавляет токен в черный список, чтобы его больше нельзя было использовать.


<h1> <strong>resource.py:</strong> </h1>

Получение ресурсов (/resources/): Возвращает список всех ресурсов, доступных в системе.

Создание ресурса (/resources/): Позволяет администраторам добавлять новые ресурсы в систему.

Обновление ресурса (/resources/{resource_id}): Позволяет администраторам обновлять информацию о существующих ресурсах.

Удаление ресурса (/resources/{resource_id}): Позволяет администраторам удалять ресурсы из системы.

Если пользователь ,не имеющий прав администратора, захочет отработать по эндпоинту, требующий от себя прав админа, то ему выдаст ошибку.


<h1> <strong>schedule.py:</strong> </h1>

Получение расписания (/schedule/): Возвращает список всех заблокированных временных интервалов для ресурсов.

Блокировка ресурса (/schedule/block): Позволяет администраторам блокировать время для ресурсов, чтобы они не могли быть забронированы в это время.



<h1> <strong>booking.py:</strong> </h1>

Создание бронирования (/bookings/): Позволяет пользователям создавать новые бронирования для ресурсов. Проверяет на пересечение с существующими бронированиями.

Получение всех бронирований (/bookings/): Позволяет администраторам получать список всех бронирований.

Получение собственных бронирований (/bookings/my): Позволяет пользователям просматривать свои собственные бронирования.

Подтверждение бронирования (/bookings/{booking_id}/confirm): Позволяет администраторам подтверждать бронирования.

Отмена бронирования (/bookings/{booking_id}): Позволяет пользователям отменять свои бронирования или администраторам отменять любые бронирования.


<h1> <strong>Удобство роутеров и их использование:</strong> </h1>


Роутеры (APIRouter) в FastAPI помогают организовать код и разделить функциональность на логические модули. Вот как роутеры используются в этом проекте:


Модульность: Каждый роутер (например, resource_router, schedule_router, booking_router, user_router) отвечает за отдельный аспект функциональности API. Это делает код более организованным и легко поддерживаемым.

Чистота кода: Каждый роутер содержит только ту логику, которая относится к его функциональности, что делает код более читаемым и проще в понимании. Этот подход обеспечивает гибкость и масштабируемость.

