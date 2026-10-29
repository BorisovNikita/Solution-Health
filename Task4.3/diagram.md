```mermaid

sequenceDiagram

&#x20;   autonumber

&#x20;   actor User as Пациент

&#x20;   participant App as Мобильное приложение

&#x20;   participant Auth as Сервер авторизации (OIDC)

&#x20;   participant GW as API Gateway



&#x20;   User->>App: Ввод логина (телефон) и пароля / SMS

&#x20;   App->>Auth: Запрос на авторизацию (с PKCE code\_challenge)

&#x20;   Auth-->>User: Экран ввода пароля / OTP

&#x20;   User->>Auth: Подтверждение

&#x20;   Auth-->>App: Authorization Code (одноразовый)

&#x20;   

&#x20;   App->>Auth: Обмен кода на токены (code + code\_verifier)

&#x20;   Auth-->>App: Access Token (JWT, 15 мин) + Refresh Token (30 дней)

&#x20;   

&#x20;   Note over App: Хранение Access Token в памяти,<br>Refresh Token в Secure Storage (Keychain/Keystore)

&#x20;   

&#x20;   App->>GW: GET /api/v1/appointments (Заголовок: Bearer <Access Token>)

&#x20;   GW->>GW: Валидация подписи JWT (публичный ключ)

&#x20;   GW-->>App: 200 OK (Данные)

