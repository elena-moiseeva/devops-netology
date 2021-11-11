1.
Воспользуемся командой git  show aefea , которая найдёт нужный хеш , покажет его полное имя и комментарии коммита


Ответ: 
	хеш- commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545


	комментарии коммита- Update CHANGELOG.md


2.


Для определения тега также воспользуемся командой git show 85024d3


Ответ:  коммиту 85024d3 соответствует тег  tag: v0.12.23



3.

С помощью команды git show b8d720^ находим первого родителя

Также добавив к команде цифру 2 и узнаем имя второго родителя 
	
Ответ: 
	у коммита b8d720 два родителя 
	
	Имя первого родителя 56cd7859e05c36c06b56d013b55a252d0bb7e158 

	Имя второго родителя 9ea88f22fc6269854151c571162c5bcf958bee2b


4.

Воспользовавшись командой git log --pretty=format:'%h %s' --graph v0.12.24,
можно увидеть кусок дерева с тегами  v0.12.23 и v0.12.24 между которыми будут расположены хеши с комментариями .

Ответ: хеши между тегами v0.12.23 и v0.12.24


	b14b74c49 [Website] vmc provider links
	
	3f235065b Update CHANGELOG.md
	
	6ae64e247 registry: Fix panic when server is unreachable
	
	5c619ca1b website: Remove links to the getting started guide's old location
	
	06275647e Update CHANGELOG.md
 	
	d5f9411f5 command: Fix bug when using terraform login on Windows
 	
	4b6d06cc5 Update CHANGELOG.md
 	
	dd01a3507 Update CHANGELOG.md
 	
	225466bc3 Cleanup after v0.12.23 release




5.
Воспользовавшись командой  git grep --heading 'func providerSource' я узнала название файла, где произошло совпадение.
Название файла: provider_source.go



Ответ: имя коммита-  commit 5af1e6234ab6da412fb8637393c5a17a1b293663



6.
Воспользовавшись командой  git grep --heading ' globalPluginDirs ' я узнала название файлов где произошло совпадение.
Название файла:
1) commands.go
2) plugins.go



Далее я применила команду git log -L :globalPluginDirs:commands.go
Указав в ней имя файла где нужно искать, и название функции



Git сообщает, что начиная с строки 1: нет совпадений

Проверяю второй файл git log -L :globalPluginDirs:plugins.go



Из полученного результата видно что в двух коммитах  была изменена функция globalPluginDirs


Ответ:

	 commit 78b12205587fe839f10d946ea3fdc06719decb05

 	 commit 78b12205587fe839f10d946ea3fdc06719decb05



7.

Воспользовавшись командой  git log -SsynchronizedWriters –oneline я  узнала хеш коммитов

Теперь с помощью команды git show bdfea50cc определим автора функции synchronizedWriters

Ответ: автор James Bardin







