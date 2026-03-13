# Policy Rules (policy-as-code): [Название проекта]

> Пример архитектурных политик в виде кода. Адаптируйте под ваш проект.

## Обзор

Политики проверяются автоматически при `terraform plan` / `kubectl apply` и блокируют развёртывание при нарушении.

## Инфраструктурные политики (OPA/Rego)

### Все хранилища должны быть зашифрованы

```rego
package terraform.storage

deny[msg] {
    resource := input.resource_changes[_]
    resource.type == "yandex_storage_bucket"
    not resource.change.after.server_side_encryption_configuration
    msg := sprintf("Бакет '%s' должен иметь шифрование at rest", [resource.name])
}
```

### Все ресурсы должны иметь обязательные теги

```rego
package terraform.tagging

required_tags := {"project", "environment", "owner", "cost-center"}

deny[msg] {
    resource := input.resource_changes[_]
    tags := object.get(resource.change.after, "tags", {})
    missing := required_tags - {key | tags[key]}
    count(missing) > 0
    msg := sprintf("Ресурс '%s' не содержит обязательные теги: %v", [resource.name, missing])
}
```

### Запрещены публичные IP без явного разрешения

```rego
package terraform.network

deny[msg] {
    resource := input.resource_changes[_]
    resource.type == "yandex_compute_instance"
    resource.change.after.network_interface[_].nat
    not resource.change.after.labels["public-access-approved"]
    msg := sprintf("Инстанс '%s' имеет публичный IP без одобрения", [resource.name])
}
```

## Kubernetes-политики (Conftest)

### Запрещён запуск контейнеров от root

```rego
package kubernetes.security

deny[msg] {
    input.kind == "Deployment"
    container := input.spec.template.spec.containers[_]
    not container.securityContext.runAsNonRoot
    msg := sprintf("Контейнер '%s' должен запускаться как non-root", [container.name])
}
```

### Обязательные resource limits

```rego
package kubernetes.resources

deny[msg] {
    input.kind == "Deployment"
    container := input.spec.template.spec.containers[_]
    not container.resources.limits.memory
    msg := sprintf("Контейнер '%s' должен иметь memory limits", [container.name])
}
```

## CI интеграция (GitLab CI)

```yaml
policy-check:
  stage: validate
  script:
    - terraform plan -out=plan.tfplan
    - terraform show -json plan.tfplan > plan.json
    - conftest test plan.json --policy policies/terraform/
  rules:
    - if: $CI_MERGE_REQUEST_ID
      changes:
        - "terraform/**"

k8s-policy-check:
  stage: validate
  script:
    - conftest test k8s/ --policy policies/kubernetes/
  rules:
    - if: $CI_MERGE_REQUEST_ID
      changes:
        - "k8s/**"
```
