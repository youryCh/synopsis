## Добавить бейдж action status

- во вкладке Actions справа наверху `...` -> `Create status badge`  
- скопировать md-разметку
- вставить в README.md
___

## Error: service exist

Ошибка: Service exists and cannot be imported into the current release: invalid ownership metadata

Чтобы починить нужно удалить deployment, ingress, service в соответствующих вкладках Kubernetes (3 отчки справа -> Delete),  
затем редеплоить приложение.
___


