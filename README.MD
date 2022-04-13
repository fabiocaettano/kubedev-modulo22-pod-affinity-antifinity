<h1>Gerenciamento da Distribuição de Pods</h1>

<h1>Pod Affinit e Antiaffinity</h1>

Regras de afinidade entre os pods.

Exemplo:
1. um pod no mesmo nó onde está sendo executado o Redis, para diminuir a latência;
2. Cada Pod seja executado em um node como uma regra de preferência para garantir alta disponibilidade.


<h3>Teste 1</h3>

Os pods do manifesto no nginx_a.yanl serão criados no mesmo node dos pods do manifesto nginx_b.yaml.

Trecho do manifesto:

``` yaml
      affinity:
        podAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: app
                    operator: In
                    values:
                      - "nginx-bbb"
              topologyKey: prioridade
```

<h3>Teste 2</h3>

Alterar replicas do nginx_a: 
``` yaml
spec:
  replicas: 6
```

Alterar replicas do nginx_b: 
``` yaml
spec:
  replicas: 2
```

Aumentará o número de replica do nginx_a devido ao aumento de réplica do nginx_b.



<h3>Teste 3</h3>

Os pods do nginx_a não serão criados no node onde estão nginx_b

``` yaml
affinity:
    podAntiAffinity:          
        requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: app
                    operator: In
                    values:
                      - "nginx-bbb"
              topologyKey: prioridade
```