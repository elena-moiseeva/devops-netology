1.	Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.

Воспользуемся командой git  show aefea , которая найдёт нужный хеш , покажет его полное имя и комментарии коммита

Результат:
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400

    Update CHANGELOG.md

diff --git a/CHANGELOG.md b/CHANGELOG.md
index 86d70e3e0..588d807b1 100644
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@ -27,6 +27,7 @@ BUG FIXES:
 * backend/s3: Prefer AWS shared configuration over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))
 * backend/s3: Prefer ECS credentials over EC2 metadata credentials by default ([#25134](https://github.com/hashicorp/terraform/issues/25134))
 * backend/s3: Remove hardcoded AWS Provider messaging ([#25134](https://github.com/hashicorp/terraform/issues/25134))
+* command: Fix bug with global `-v`/`-version`/`--version` flags introduced in 0.13.0beta2 [GH-25277]
 * command/0.13upgrade: Fix `0.13upgrade` usage help text to include options ([#25127](https://github.com/hashicorp/terraform/issues/25127))
 * command/0.13upgrade: Do not add source for builtin provider ([#25215](https://github.com/hashicorp/terraform/issues/25215))
 * command/apply: Fix bug which caused Terraform to silently exit on Windows when using absolute plan path ([#25233](https://github.com/hashicorp/terraform/issues/25233))


Ответ: 
	хеш- commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545

	комментарии коммита- Update CHANGELOG.md





2.	Какому тегу соответствует коммит 85024d3?


Для определения тега также воспользуемся командой git show 85024d3
Результат:
$ git show 85024d3
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Thu Mar 5 20:56:10 2020 +0000

    v0.12.23

diff --git a/CHANGELOG.md b/CHANGELOG.md
index 1a9dcd0f9..faedc8bf4 100644
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@ -1,4 +1,4 @@
-## 0.12.23 (Unreleased)
+## 0.12.23 (March 05, 2020)
 ## 0.12.22 (March 05, 2020)

 ENHANCEMENTS:
diff --git a/version/version.go b/version/version.go
index 33ac86f5d..bcb6394d2 100644
--- a/version/version.go
+++ b/version/version.go
@@ -16,7 +16,7 @@ var Version = "0.12.23"
 // A pre-release marker for the version. If this is "" (empty string)
 // then it means that it is a final release. Otherwise, this is a pre-release
 // such as "dev" (in development), "beta", "rc1", etc.
-var Prerelease = "dev"
+var Prerelease = ""

 // SemVer is an instance of version.Version. This has the secondary
 // benefit of verifying during tests and init time that our version is a


Ответ: 

	коммиту 85024d3 соответствует тег  tag: v0.12.23




3.	Сколько родителей у коммита b8d720? Напишите их хеши.
С помощью команды git show b8d720^ находим первого родителя

Результат:

 $ git show b8d720^
commit 56cd7859e05c36c06b56d013b55a252d0bb7e158
Merge: 58dcac4b7 ffbcf5581
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Mon Jan 13 13:19:09 2020 -0800

    Merge pull request #23857 from hashicorp/cgriggs01-stable

    [cherry-pick]add checkpoint links

Также добавив к команде цифру 2 и узнаем имя второго родителя 
git show b8d720^2
commit 9ea88f22fc6269854151c571162c5bcf958bee2b
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Tue Jan 21 17:08:06 2020 -0800

    add/update community provider listings

diff --git a/website/docs/providers/type/community-index.html.markdown b/website/docs/providers/type/community-index.html.markdown
index 6119f048f..675059dd4 100644
--- a/website/docs/providers/type/community-index.html.markdown
+++ b/website/docs/providers/type/community-index.html.markdown
@@ -28,19 +28,23 @@ please fill out this [community providers form](https://docs.google.com/forms/d/
 - [Apigee](https://github.com/zambien/terraform-provider-apigee)
 - [Artifactory](https://github.com/atlassian/terraform-provider-artifactory)
 - [Auth](https://github.com/Shuttl-Tech/terraform-provider-auth)
-- [Auth0](https://github.com/bocodigitalmedia/terraform-provider-auth0)
+- [Auth0](https://github.com/alexkappa/terraform-provider-auth0)
 - [Automic Continuous Delivery](https://github.com/Automic/terraform-provider-cda)
 - [AVI](https://github.com/avinetworks/terraform-provider-avi)
 - [Aviatrix](https://github.com/AviatrixSystems/terraform-provider-aviatrix)
 - [AWX](https://github.com/mauromedda/terraform-provider-awx)
 - [Azure Devops](https://github.com/agarciamiravet/terraform-provider-azuredevops)
 - [Bitbucket Server](https://github.com/gavinbunney/terraform-provider-bitbucketserver)
+- [CDS](https://github.com/capitalonline/terraform-provider-cds)
	

Ответ: 

	у коммита b8d720 два родителя 

	Имя первого родителя 56cd7859e05c36c06b56d013b55a252d0bb7e158 

	Имя второго родителя 9ea88f22fc6269854151c571162c5bcf958bee2b






4.	Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

Воспользовавшись командой git log --pretty=format:'%h %s' --graph v0.12.24,
можно увидеть кусок дерева с тегами  v0.12.23 и v0.12.24 между которыми будут расположены хеши с комментариями .
Результат:
git log --pretty=format:'%h %s' --graph v0.12.24
* 33ff1c03b v0.12.24
* b14b74c49 [Website] vmc provider links
* 3f235065b Update CHANGELOG.md
* 6ae64e247 registry: Fix panic when server is unreachable
* 5c619ca1b website: Remove links to the getting started guide's old location
* 06275647e Update CHANGELOG.md
* d5f9411f5 command: Fix bug when using terraform login on Windows
* 4b6d06cc5 Update CHANGELOG.md
* dd01a3507 Update CHANGELOG.md
* 225466bc3 Cleanup after v0.12.23 release
* 85024d310 v0.12.23

Ответ: 
	b14b74c49 [Website] vmc provider links
	
	3f235065b Update CHANGELOG.md
	
	6ae64e247 registry: Fix panic when server is unreachable
	
	5c619ca1b website: Remove links to the getting started guide's old location
	
	06275647e Update CHANGELOG.md
 	
	d5f9411f5 command: Fix bug when using terraform login on Windows
 	
	4b6d06cc5 Update CHANGELOG.md
 	
	dd01a3507 Update CHANGELOG.md
 	
	225466bc3 Cleanup after v0.12.23 release





5.	Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).
	
	Воспользовавшись командой  git grep --heading 'func providerSource' я узнала название файла, где произошло совпадение.
Название файла: provider_source.go

Результат:
$ git grep --heading 'func providerSource'
provider_source.go
func providerSource(configs []*cliconfig.ProviderInstallation, services *disco.Disco) (getproviders.Source, tfdiags.Diagnostics) {
func providerSourceForCLIConfigLocation(loc cliconfig.ProviderInstallationLocation, services *disco.Disco) (getproviders.Source, tfdiags.Diagnostics) {

Далее я применила команду git log -L :'func providerSource':provider_source.go
Указав в ней имя файла где нужно искать, и название функции.

git log -L :'func providerSource':provider_source.go
commit 5af1e6234ab6da412fb8637393c5a17a1b293663
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Tue Apr 21 16:28:59 2020 -0700

    main: Honor explicit provider_installation CLI config when present

    If the CLI configuration contains a provider_installation block then we'll
    use the source configuration it describes instead of the implied one we'd
    build otherwise.

diff --git a/provider_source.go b/provider_source.go
--- a/provider_source.go
+++ b/provider_source.go
@@ -20,6 +23,15 @@
-func providerSource(services *disco.Disco) getproviders.Source {
-       // We're not yet using the CLI config here because we've not implemented
-       // yet the new configuration constructs to customize provider search
-       // locations. That'll come later. For now, we just always use the
-       // implicit default provider source.
-       return implicitProviderSource(services)
+func providerSource(configs []*cliconfig.ProviderInstallation, services *disco.Disco) (getproviders.Source, tfdiags.Diagnostics) {
+       if len(configs) == 0 {
+               // If there's no explicit installation configuration then we'll build
+               // up an implicit one with direct registry installation along with
+               // some automatically-selected local filesystem mirrors.
+               return implicitProviderSource(services), nil
+       }
+
+       // There should only be zero or one configurations, which is checked by


Ответ: 

имя коммита-  commit 5af1e6234ab6da412fb8637393c5a17a1b293663




6.	Найдите все коммиты в которых была изменена функция globalPluginDirs.

	Воспользовавшись командой  git grep --heading ' globalPluginDirs ' я узнала название файлов где произошло совпадение.
Название файла:

1) commands.go

2) plugins.go


$ git grep --heading 'globalPluginDirs'
commands.go
                GlobalPluginDirs: globalPluginDirs(),
        helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
internal/command/cliconfig/config_unix.go
                // FIXME: homeDir gets called from globalPluginDirs during init, before
plugins.go
// globalPluginDirs returns directories that should be searched for
func globalPluginDirs() []string {

Далее я применила команду git log -L :globalPluginDirs:commands.go
Указав в ней имя файла где нужно искать, и название функции


git log -L :globalPluginDirs:commands.go
fatal: -L parameter 'globalPluginDirs' starting at line 1: no match
Git сообщает, что начиная с строки 1: нет совпадений

Проверяю второй файл git log -L :globalPluginDirs:plugins.go


git log -L :globalPluginDirs:plugins.go
commit 78b12205587fe839f10d946ea3fdc06719decb05
Author: Pam Selle <204372+pselle@users.noreply.github.com>
Date:   Mon Jan 13 16:50:05 2020 -0500

    Remove config.go and update things using its aliases

diff --git a/plugins.go b/plugins.go
--- a/plugins.go
+++ b/plugins.go
@@ -16,14 +18,14 @@
 func globalPluginDirs() []string {
        var ret []string
        // Look in ~/.terraform.d/plugins/ , or its equivalent on non-UNIX
-       dir, err := ConfigDir()
+       dir, err := cliconfig.ConfigDir()
        if err != nil {
                log.Printf("[ERROR] Error finding global config directory: %s", err)
        } else {
                machineDir := fmt.Sprintf("%s_%s", runtime.GOOS, runtime.GOARCH)
                ret = append(ret, filepath.Join(dir, "plugins"))
                ret = append(ret, filepath.Join(dir, "plugins", machineDir))
        }

        return ret
 }

commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46
Author: James Bardin <j.bardin@gmail.com>
Date:   Wed Aug 9 17:46:49 2017 -0400


Из полученного результата видно что в двух коммитах  была изменена функция globalPluginDirs


Ответ: 

	commit 78b12205587fe839f10d946ea3fdc06719decb05

	commit 78b12205587fe839f10d946ea3fdc06719decb05




7.	Кто автор функции synchronizedWriters?

	Воспользовавшись командой  git log -SsynchronizedWriters –oneline я  узнала хеш коммитов
git log -SsynchronizedWriters --oneline
bdfea50cc remove unused
fd4f7eb0b remove prefixed io
5ac311e2a main: synchronize writes to VT100-faker on Windows

Теперь с помощью команды git show bdfea50cc определим автора функции synchronizedWriters
git show bdfea50cc
commit bdfea50cc85161dea41be0fe3381fd98731ff786
Author: James Bardin <j.bardin@gmail.com>
Date:   Mon Nov 30 18:02:04 2020 -0500

Ответ: автор James Bardin

