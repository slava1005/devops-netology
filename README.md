# devops-netology
<<<<<<< HEAD
# Задание 3 Ветки
=======
# USE TOKENS!!!
>>>>>>> main
# Работа над ошибками 26022023

Файл .gitignore:
# Исключит все файлы .tfvars, которые могут содержать конфиденциальные данные, такие как
# пароль, закрытые ключи и другие секреты. Они не должны быть частью версии 
# контролируйте, поскольку они являются точками данных, которые потенциально чувствительны и подвержены 
# для изменения в зависимости от среды.
# Прогнорируeт файлы переопределения, поскольку они обычно используются для переопределения ресурсов локально и поэтому
# не зарегистрированы
# Включит переопределенные файлы, которые вы хотите добавить в систему управления версиями, используя отрицаемый шаблон
# !example_override.tf
# Чтобы игнорировать вывод плана команды: terraform plan -out=tfplan, включите файлы tfplan
# пример: * tfplan*

# Прогнорирует файлы конфигурации CLI
