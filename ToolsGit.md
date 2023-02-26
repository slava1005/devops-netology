Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.
root@debian:/terraform/terraform2# git show aefea
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400

    Update CHANGELOG.md

diff --git a/CHANGELOG.md b/CHANGELOG.md
index 86d70e3e0d..588d807b17 100644
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

Ответьте на вопросы.
Какому тегу соответствует коммит 85024d3?
root@debian:/terraform/terraform2# git tag -l --points-at 85024d3
v0.12.23

Сколько родителей у коммита b8d720? Напишите их хеши.
root@debian:/terraform/terraform2# git show --pretty=%P b8d720
Родителей два, их хеши
56cd7859e05c36c06b56d013b55a252d0bb7e158 и 9ea88f22fc6269854151c571162c5bcf958bee2b

Перечислите хеши и комментарии всех коммитов, которые были сделаны между тегами v0.12.23 и v0.12.24.
Между тегами 10 коммитов. Хеши и комментарии:
root@debian:/terraform/terraform2# git log v0.12.23..v0.12.24 --oneline
33ff1c03bb (tag: v0.12.24) v0.12.24
b14b74c493 [Website] vmc provider links
3f235065b9 Update CHANGELOG.md
6ae64e247b registry: Fix panic when server is unreachable
5c619ca1ba website: Remove links to the getting started guide's old location
06275647e2 Update CHANGELOG.md
d5f9411f51 command: Fix bug when using terraform login on Windows
4b6d06cc5d Update CHANGELOG.md
dd01a35078 Update CHANGELOG.md
225466bc3e Cleanup after v0.12.23 release
root@debian:/terraform/terraform2#

Найдите коммит, в котором была создана функция func providerSource, её определение в коде выглядит так: func providerSource(...) 
(вместо троеточия перечислены аргументы).
Функция func providerSource была создана в коммите 8c928e8358 от 02.04.2020
root@debian:/terraform/terraform2# git grep -n 'func providerSource(.*)'
provider_source.go:23:func providerSource(configs []*cliconfig.ProviderInstallation, services *disco.Disco) (getproviders.Source, tfdiags.Diagnostics) {

root@debian:/terraform/terraform2# git log --oneline -S'func providerSource' provider_source.go
5af1e6234a main: Honor explicit provider_installation CLI config when present
8c928e8358 main: Consult local directories as potential mirrors of providers

git checkout 8c928e835

root@debian:/terraform/terraform2# git grep -n 'func providerSource(.*)'
provider_source.go:19:func providerSource(services *disco.Disco) getproviders.Source {

8c928e83589 (Martin Atkins 2020-04-02 18:04:39 -0700 19) func providerSource(services *disco.Disco) getproviders.Source {

Найдите все коммиты, в которых была изменена функция globalPluginDirs.
Функцию globalPluginDirs исправляли в пяти коммитах.
root@debian:/terraform/terraform2# git grep 'func globalPluginDirs(.*)'
plugins.go:func globalPluginDirs() []string {

root@debian:/terraform/terraform2# git log -L :globalPluginDirs:plugins.go  -s --oneline
78b1220558 Remove config.go and update things using its aliases
52dbf94834 keep .terraform.d/plugins for discovery
41ab0aef7a Add missing OS_ARCH dir to global plugin paths
66ebff90cd move some more plugin search path logic to command
8364383c35 Push plugin discovery down into command package

Кто автор функции synchronizedWriters?
Автор функции synchronizedWriters Martin Atkins. Он добавил её 2017-05-03 коммитом 5ac311e2a

Примерно в то же время, когда Андрей записывал курс, James Bardin удалил эту функцию коммитом bdfea50cc от Mon Nov 30 18:02:04 2020 с комментарием remove unused.

$ git log -S'func synchronizedWriters' --oneline
bdfea50cc remove unused
5ac311e2a main: synchronize writes to VT100-faker on Windows
git checkout 5ac311e2a

$ git grep -n 'func synchronizedWriters(.*)'
synchronized_writers.go:15:func synchronizedWriters(targets ...io.Writer) []io.Writer {
$ git blame -L 15,15 synchronized_writers.go
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 15) func synchronizedWriters(targets ...io.Writer) []io.Writer {



