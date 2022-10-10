# Создание веток и переключение
`git branch`: отображает все существующие ветки и с помощью знака '*' (указателя HEAD) показывает текущую ветку (на которой сейчас мы находимся).

`git branch branchName`: создаёт новую ветку с именем 'branchName'.

`git log --oneline --decorate --graph --all`: отображает историю коммитов, текущее положение указателей веток и историю ветвления.

`git checkout branchName` / `git switch branchName`: переключает на ветку с именем 'branchName'.

`git checkout -b branchName` / `git switch -с branchName`:  создаёт новую ветку с название 'branchName' и сразу переключается на неё. 

`git switch -`: вернуться к предыдущей извлечённой ветке.

# Слияние веток
`git merge branchName`: производит слияние ветки branchName с текущей веткой (на которой находитесь).

`git mergetool`: вызывает графический инструмент для разрешения конфликтов слияния.

# Управление ветками
`git branch -v`:  позволяет посмотреть последний коммит на каждой из веток.

`git branch -d branchName`:  удаление ветки (выведет ошибку если на этой ветке есть не смёрженные наработки).

`git branch -D branchName`:  удаление ветки даже если на этой ветке есть не смёрженные наработки.

`git branch --move bad-branch-name corrected-branch-name`:  ветка 'bad-branch-name' будет переименована в 'corrected-branch-name', после переименования отправляем исправленную ветку в удалённый репозиторий следующей командой: `git push --set-upstream origin corrected-branch-name`, после чего удаляем старую ветку на удалённом сервере с помощью команды `git push origin --delete bad-branch-name`.

# Удалённые ветки

