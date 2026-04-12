<p align="center">
  <img src="../banner.png" width="100%" alt="BobiVPN Banner"/>
</p>

<h1 align="center">🐹 VPN Checker — Go Edition 🐹</h1>

<p align="center">
  <b>Сверхбыстрая проверка VPN ключей с Xray-core</b><br>
  <i>Двухступенчатая проверка • Xray-core библиотека • Полная валидация</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Go-🐹-00ADD8?style=for-the-badge" alt="Go">
  <img src="https://img.shields.io/badge/Speed-⚡_Fast-green?style=for-the-badge" alt="Speed">
  <img src="https://img.shields.io/badge/Xray--core-v26.3.27-purple?style=for-the-badge" alt="Xray-core">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/VLESS-Reality%20%7C%20TLS%20%7C%20WS-blue?style=flat-square" alt="VLESS">
  <img src="https://img.shields.io/badge/VMess-WebSocket%20%7C%20gRPC-green?style=flat-square" alt="VMess">
  <img src="https://img.shields.io/badge/Trojan-TLS-orange?style=flat-square" alt="Trojan">
  <img src="https://img.shields.io/badge/Shadowsocks-AEAD-red?style=flat-square" alt="SS">
</p>

---

## ⚡ Почему Go + Xray-core?

<table>
<tr>
<td width="50%" valign="top">

### 🦀 Rust + sing-box
- Очень быстрый
- Требует sing-box в PATH
- Отдельный процесс для каждой проверки
- 1-2 часа на 32K ключей

</td>
<td width="50%" valign="top">

### 🐹 Go + Xray-core
- Быстрый и эффективный
- Xray-core как библиотека (в памяти)
- Нет внешних зависимостей
- Параллельность: 300 проверок
- Полная совместимость с Xray

</td>
</tr>
</table>

> [!TIP]
> **Xray-core как библиотека** — быстрее и надёжнее чем subprocess!

---

## 🚀 Быстрый старт

### Скачать готовый бинарник

```bash
# Linux
wget https://github.com/YOUR_USERNAME/YOUR_REPO/releases/download/latest/v2go-checker-linux-amd64
chmod +x v2go-checker-linux-amd64
./v2go-checker-linux-amd64
```

### Использование

```bash
# Создай файл subscriptions.txt с URL подписок
echo "https://example.com/subscription.txt" > subscriptions.txt

# Запусти проверку
./v2go-checker
```

---

## 📁 Структура

```
v2go_checker_bin/
├── .github/
│   └── workflows/
│       └── v2go-checker.yml  # 🔄 GitHub Actions
├── scripts/
│   └── v2go-checker          # 🐹 Скомпилированный бинарник
├── subscriptions.txt         # 📋 Список URL подписок
└── README.md                 # 📖 Документация
```

---

## 🔧 Как это работает

```mermaid
graph LR
    A[📥 Загрузка] --> B[🔌 TCP Ping]
    B --> C[⚙️ Xray-core]
    C --> D[🌍 IP Check]
    D --> E[📊 Speed Test]
    E --> F[✅ Рабочие]
    F --> G[📤 Публикация]
    
    style A fill:#4CAF50,color:#fff
    style F fill:#2196F3,color:#fff
    style G fill:#9C27B0,color:#fff
```

### Двухступенчатая проверка:

| Этап | Описание | Время |
|------|----------|-------|
| **Этап 1: Фильтрация** | TCP ping всех ключей | ~30 сек |
| **Этап 2: Полная проверка** | Xray → IP → Speed | ~2-5 мин |

#### Этап 2 включает:
1. **TCP Ping** — Проверка доступности порта (3 сек таймаут)
2. **Xray Instance** — Запуск Xray-core в памяти
3. **Connectivity** — Проверка соединения через прокси
4. **IP Check** — Проверка что exit IP изменился
5. **GeoIP** — Определение страны и ISP
6. **Speed Test** — Измерение скорости загрузки

---

## 📊 Выходные файлы

| Файл | Описание |
|------|----------|
| `vpn.txt` | Оригинальные рабочие ключи |
| `vpn_renamed.txt` | С красивыми именами (🇷🇺 Russia \| Yandex 1) |
| `bobi_vpn.txt` | Для Happ (с заголовком подписки) |
| `bobi_vpn_lite.txt` | Lite версия (RU/DE/FR/FI/EE/LV/LT) |
| `vpn_report.json` | JSON отчёт с метаданными |
| `countries/*.txt` | Подписки по отдельным странам |

### Формат имён:

```
🇷🇺 Russia | Yandex 1
🇩🇪 Germany | Hetzner 2
🇫🇷 France | OVH 1
```

---

## ⚙️ Конфигурация

Настройки в коде (в `checker/full_check.go`):

```go
timeout:       15 * time.Second  // Таймаут проверки
maxConcurrent: 300               // Параллельных проверок
portStart:     10000             // Начальный порт для Xray
portEnd:       15000             // Конечный порт
```

Настройки TCP ping (в `checker/checker.go`):

```go
TCPTimeout:    3 * time.Second   // Таймаут TCP пинга
MaxLatency:    2000              // Максимальный пинг (мс)
```

---

## 🔨 Сборка из исходников

### Требования
- Go 1.26+
- Xray-core v1.260327.0 (автоматически скачается)

### Сборка

```bash
cd ../v2go-main
GOOS=linux GOARCH=amd64 go build -ldflags="-s -w" -o bin/v2go-checker main.go output.go sort.go
cp bin/v2go-checker ../v2go_checker_bin/scripts/
```

---

## 🌍 Поддерживаемые страны

<p align="center">
🇷🇺 Россия • 🇩🇪 Германия • 🇳🇱 Нидерланды • 🇫🇮 Финляндия • 🇸🇪 Швеция<br>
🇵🇱 Польша • 🇫🇷 Франция • 🇬🇧 Великобритания • 🇺🇸 США • 🇰🇿 Казахстан<br>
🇱🇹 Литва • 🇱🇻 Латвия • 🇪🇪 Эстония • 🇨🇭 Швейцария • 🇦🇹 Австрия<br>
🇹🇷 Турция • 🇮🇱 Израиль • 🇯🇵 Япония • 🇸🇬 Сингапур • 🇭🇰 Гонконг
</p>

---

## ⚠️ Требования

- **subscriptions.txt** с URL подписок (по одному на строку)
- **Нет внешних зависимостей** — Xray-core встроен в бинарник!

---

## 🆚 Сравнение с другими версиями

| Параметр | Python | Rust + sing-box | Go + Xray-core |
|----------|--------|-----------------|----------------|
| **Скорость** | 32-53 часа | 1-2 часа | 2-5 минут |
| **Параллельность** | 50 | 1000+ | 300 |
| **Зависимости** | Python + sing-box | sing-box | Нет |
| **Память** | ~2GB | ~500MB | ~800MB |
| **Интеграция** | Subprocess | Subprocess | Library |

---

## 📝 Примечания

- Бинарник содержит Xray-core v26.3.27 (последняя версия)
- Поддержка всех протоколов: VLESS (Reality/TLS), VMess, Trojan, Shadowsocks
- Автоматическое определение страны и ISP через GeoIP
- Красивые имена с флагами и номерами

---

<p align="center">
  <b>⭐ Поставь звезду если полезно!</b>
</p>

<p align="center">
  <sub>Made with 🐹 by BobiVPN Team</sub>
</p>
