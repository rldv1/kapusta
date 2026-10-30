# kapusta

[English](README.en.md)

Крайне сырая и экспериментальная монолитная базовая станция 2G + 3G + LTE + NR + CDMA + NB-IoT, которая работает на 1T1R USRP B210.

Больше информации будет позже.

## Требования

| | Минимум |
|---|---|
| Радио | Ettus USRP B210 (или B200/B200mini) на USB 3 |
| Опорный сигнал | GPSDO с выходами 10 МГц + PPS (обязателен) |
| CPU | Производительный x86-64 с AVX2/FMA, 8 физических ядер для полного набора RAT, например Ryzen 7-го поколения и выше |
| ОС | Linux x86-64 (glibc 2.39+), рекомендуется PREEMPT_RT или lowlatency ядро |
| SIM | [Программируемые USIM (Milenage K/OPc)](https://www.gialer.com/collections/sim-card-program-kit-1/products/gialer-sim-card-program-kit-sim-card-tools-accessories-include-1-multi-sim-card-reader-5pcs-programmable-usim-cards-3-in-1-card-adapter-kit-newest-grsim-software-programer-tool) или [программируемая eSIM](https://simlessly.com) |

### GPSDO
Для работы требуется GPSDO, без него работа проекта будет нестабильна, я рекомендую [вот этот](https://www.leobodnar.com/shop/index.php?main_page=product_info&cPath=107&products_id=393).

## Запуск

Ничего ставить не нужно: радиостеки, опорная сеть, IMS, база абонентов и веб-панель уже внутри одного файла.

1. Подключите аттенюаторы или антенны, заведите 10 МГц + PPS от GPSDO на USRP и подключите USRP к USB 3.0.
2. Скачайте последний бинарник со страницы [Releases](../../releases) и запустите его от root:
   ```sh
   wget https://github.com/rldv1/kapusta/releases/download/v1.0.0/kpstv1.0.0-linux-amd64
   chmod +x kpstv1.0.0-linux-amd64
   sudo ./kpstv1.0.0-linux-amd64
   ```
3. При первом запуске проведите первоначальную настройку! В консоли отобразится QR-код с панелью админа и паролем. Настройте сеть и добавьте абонентов.
4. Нажмите Ready, когда составите карту частот **(не перекрывайте частоты реальных операторов!)**, после чего начнется эфир.

Все настройки, база абонентов и логи лежат в папке `kapusta-data/` рядом с бинарником, больше ничего на систему не ставится. Чтобы перенести ноду на другой компьютер, скопируйте бинарник вместе с этой папкой. Остановить: `Ctrl+C` или `stop` в консоли.

### Консоль оператора

| Команда | Действие |
|---|---|
| `status`, `health`, `ue list` | Активные RAT, захват USRP/GPSDO, загрузка CPU, подключённые телефоны |
| `rat on <rat>`, `rat off <rat>` | Включение и выключение `gsm`, `umts`, `lte`, `nr`, `nbiot`, `cdma` на ходу |
| `sub add <imsi> <k> <opc> [msisdn]`, `sub del <imsi>`, `sub list` | Абоненты в HSS, HLR и UMTS AuC |
| `spectrum`, `spectrum set <rat> <center_mhz> [<bw_mhz>\|<profile>] [restart]` | Несущие, перенос и изменение ширины одного RAT на ходу (`lte umts umts_c1 gsm cdma nbiot`) |
| `umts tx_gain <db>`, `rf gain <rat> <tx_db> [rx_db]` | Параметры радио |
| `log filter <tag...>`, `log mute <tag>`, `log unmute <tag>`, `log level info\|debug` | Фильтры лога |
| `stop`, `exit` | Остановить радио |

## Контакт

https://t.me/rldv1
