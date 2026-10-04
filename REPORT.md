Звіт до ЛР1. Контейнер розробника: перша капсула

Номер у журналі: 5
Розрахунок: 5 % 4 = 1
Суфікс контейнера: bravo
HOST_PORT: 8081


@dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ docker version

Client:
 Version:           29.8.2-1
 API version:       1.56
 Go version:        go1.27.1
 Git commit:        7fc2dff9bceb96b266a3b2c3117c0955a0d9e616
 Built:             Thu Oct  1 10:22:53 2026
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          29.8.2-1
  API version:      1.56 (minimum version 1.40)
  Go version:       go1.27.1
  Git commit:       8af9fe3a36bab3e039862a2ab1cef1880c9b4d03
  Built:            Thu Oct  1 11:00:16 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          2.4.1-1
  GitCommit:        f2551031d7276a770f65f98c9b52e57e7dad07e8
 runc:
  Version:          1.5.2-4
  GitCommit:        29dd3dc2b13b4123162e5fe132504bb4b15569f1
 docker-init:
  Version:          0.19.0
  GitCommit:  



  @dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ docker run -it --name devbox-bravo ubuntu:24.04 bash
root@a64f37958013:/# echo $$

1


root@a64f37958013:/# ps aux
...
Running hooks in /etc/ca-certificates/update.d...
done.
root@a64f37958013:/# ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   4592  4132 pts/0    Ss   16:06   0:00 bash
root        2911  0.0  0.0   7896  4220 pts/0    R+   16:08   0:00 ps aux
root@a64f37958013:/#  ps aux | wc -l

4


root@a64f37958013:/# exit

exit


@dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ docker rm -f devbox-bravo
devbox-bravo


@dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ ls -la level1.sh
-rw-rw-rw- 1 vscode vscode 587 Oct  4 16:05 level1.sh


@dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ chmod +x level1.sh


@dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ ls -la level1.sh
-rwxrwxrwx 1 vscode vscode 587 Oct  4 16:05 level1.sh


@dovzhenkosofiia-pixel ➜ /workspaces/sp-lab01-template (main) $ ./level1.sh && ./level1.sh
CONTAINER_PID=31881
CONTAINER_PROCS=4
HOST_PROCS=26
CONTAINER_PID=32037
CONTAINER_PROCS=4
HOST_PROCS=26





Відповіді на контрольні питання 

1. Чому процес усередині контейнера бачить себе як PID 1, хоча на хості в нього інший, реальний PID?

Бо контейнер має власний PID namespace — нумерація процесів починається з 1. На хості той самий процес має свій «глобальний» PID.

2. Чому ядро в контейнера те саме, що на хості, — і як це перевірити однією командою для процесу?

Контейнер — не віртуальна машина, а ізольовані процеси на спільному ядрі. Перевірити: uname -r всередині й на хості — однаково.

3. Що станеться, якщо звернутися до http://localhost:8080 з хоста, а контейнер запущено без прапорця -p?

Connection refused — порт не проброшено, NAT-правило не створене.

4. Чим прапорець -p 8080:80 відрізняється від -p 80:80 з погляду безпеки локальної машини?

Порт 80 — привілейований (потрібен root) і відкривається для всіх інтерфейсів. Порт 8080 — непривілейований, безпечніший. Для локального доступу: -p 127.0.0.1:8080:80.

5. Значення echo $$ усередині контейнера і номер того самого процесу на хості відрізняються. Це один процес чи два? 

Один процес. Різні номери — бо це два різних PID namespace.

6. Чому автоматизація розгортання bash-скриптом надійніша за ручний набір команд у терміналі щоразу?

Відтворюваний, версіонується в git, не залежить від пам'яті, ідемпотентний — можна запускати повторно без прибирання.

7. Що покаже ps aux на хості для процесу з контейнера?

Звичайний рядок із «глобальним» PID хоста і назвою команди (наприклад, bash, sleep). Позначки «це контейнер» немає.

8. Чому для ідемпотентності скрипту потрібно прибирати або перевикористовувати контейнер із заданим іменем перед повторним docker run?

Бо docker run --name X впаде з помилкою name already in use. Тому на початку — docker rm -f X.