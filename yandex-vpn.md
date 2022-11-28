## Как поднять российский VPN

Делаем на Яндексе, хотя и не все их IP-адреса подходят для Госуслуг: возможно, придётся переделывать.

[cloud.yandex.ru](https://cloud.yandex.ru) — заходим в Яндекс.Облако:
* завести аккаунт,
* [console.cloud.yandex.ru/billing](https://console.cloud.yandex.ru/billing) — подключить карту и вот это всё (или где-то можно подключить пробный период)


[console.cloud.yandex.ru](https://console.cloud.yandex.ru) → Compute Cloud → «Создать ВМ». Тут указываем:
* имя сервера: yandexvpn (или любое другое название — неважно),
* описание: «впн для меня»,
* зону доступности выбираем любую (я бы не трогал вообще параметр),
* Операционная система: `Ubuntu 18.04`: без GPU, `22.04` у меня не работает.
* Диски: 20 Гб должно хватить, HDD. Файловые хранилища нам не нужны, их не трогаем.
* Платформа — можно не трогать и оставить `Intel Ice Lake`, vCPU: 2,
а вот гарантированную долю — ставим 20% — так дешевле, RAM — 2 ГБ хватит.
* «Прерываемая» — так ещё дешевле, но будут тормоза, а ещё IP-адрес может поменяться.
* Сетевые настройки: подсеть по умолчанию, публичный адрес «автоматически»,
защита от DDOS не нужна (их не ожидаем), внутренний IPv4 — автоматически.
* Доступ: логин — ваше никнейм: например, yandexvpn, а вот дальше у себя в терминале выполняем:

```bash
$ ssh-keygen -t 'ed25519' -N '' -f "${HOME}/.ssh/id_ed25519_yandex"
$ cat "${HOME}/.ssh/id_ed25519_yandex.pub" | pbcopy
```
теперь в буфере обмена то, что надо вставить в поле SSH-ключ.

Всё, убеждаемся, что цена ок, и жмём «Создать ВМ»

Далее на странице [console.cloud.yandex.ru](https://console.cloud.yandex.ru) → `Compute Cloud`
ищем `Публичный IPv4` нового сервера: он имеет вид `XX.YYY.ZZZ.WW`, копируем:
```bash
$ ssh -i "${HOME}/.ssh/id_ed25519_yandex" "yandexvpn@XX.YYY.ZZZ.WW"
$ # теперь вы на сервере
$ mkdir -p "${HOME}/openvpn-install" && cd $_
$ curl -O 'https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh'
$ sudo bash openvpn-install.sh
```

Теперь накликиваем там всё.
1. IP address — стираем и пишем `XX.YYY.ZZZ.WW`
2. Do you want to enable IPv6 support → `n`
3. Port choice [1-3]: `1`
4. Protocol [1-2]: `1`
5. DNS [1-12]: `9` ← тут я поменял, но можно оставить
6. Enable compression? [y/n]: `n`
7. Customize encryption settings? [y/n]: `n`
8. В какой-то момент попросит создать клиента: `YYYY-MM-DD-anyname`
9. Select an option [1-2]: `1`

Готово! Чтобы посмотреть делаем: `cat ${HOME}/YYYY-MM-DD-anyname.ovpn`
Содержимое копируем себе на комп (в новый файл) и открываем файл в [Tunnelblick](https://tunnelblick.net/downloads.html).